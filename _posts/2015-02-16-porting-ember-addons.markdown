---
title: Porting Ember Addons
date: 2015-02-16 20:41:36 
tags: 
layout: post
---
I recently converted [Giovanni Collazo](https://www.twitter.com/gcollazo)'s [Ember Addons](http://www.emberaddons.com/) work to become [Broccoli Plugins](http://broccoliplugins.com/). It's these kind of things that make you realise how simple (in an elegant way, not a boring way) things can be. With excellent groundwork and foundations laid I was able to take somebody else's project and get a different version up and running over 2 days. This was also an open-source contribution, something which, I'll be honest, I'm a little scared of sometimes — there's a lot of eyes out there!

I really liked the way Giovanni tackled the problem of reading from the npm registry, and feeding that data in to an Ember app. One particular thing to note was that Ember Data wasn't used, and Giovanni had implemented his own store and model feeding. 

On the backend there was a Node.js server set up, with two purposes. One was to run an `update.js` script periodically — I chose once daily — that fetched plugins from the npm registry via keyword and saved them to a number of static `.json` files on S3, also updating a metrics total in Postgres whilst there. Secondly, to provide a top-level redirect that would take you to the `plugins.json` file, and a `/stats` endpoint to read the metrics from Postgres. All of the paging was also worked out once the data was collected, and stored in their own `pages.json` files. 

One thing this forced me to do was to re-learn some of Amazon S3 again — it'd admittedly been a while since I'd set anything up from scratch. Some nice discoveries were how easy it was to setup a policy to allow all files within a bucket to be publicly readable:

```
{
  "Statement": [
    {
      "Sid": "AllowPublicRead",
      "Effect": "Allow",
      "Principal": {
        "AWS": "*"
      },
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::<BUCKET NAME>/*"
    }
  ]
}
``` 

Also needed a CORS policy for good measure:

```
<?xml version="1.0" encoding="UTF-8"?>
<CORSConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
    <CORSRule>
        <AllowedOrigin>*</AllowedOrigin>
        <AllowedMethod>GET</AllowedMethod>
        <MaxAgeSeconds>3000</MaxAgeSeconds>
    </CORSRule>
</CORSConfiguration>
```

This is one of the beauties of single page apps in my eyes. Our backend can worry about being an API layer — of which it's very good at doing so. And a completely separate app / repo can deal with the front-end. The two pair together like cheese and biscuits and everyone is happy. This forces you to think in an entirely different way about a number of problems. I find one of these is authentication, rather than "hacking" a cookie in to your Ember (or Backbone, Angular, React etc) layer and somehow sending that with XHR requests, you are forced to use solutions such as Access Tokens. That is an entirely, and MAHOOSIVE, problem all on it's own though. 

On the front-end this feeds into a custom `store`. The store itself as well as models are injected via an initializer:

```
container.register('store:main', Store, { singleton: true });
container.register('model:package', Package, { singleton: false });
container.register('model:npm-user', NpmUser, { singleton: false });
container.register('model:github-user', GithubUser, { singleton: false });
container.register('model:github-repo', GithubRepo, { singleton: false });

application.inject('route', 'store', 'store:main');
application.inject('controller', 'store', 'store:main');
application.inject('model', 'store', 'store:main');
```

If you haven't used initializers for dependency injection it's a great way to manage a variety of things. I've had great success injecting application wide notification and modal services this way. It's also a great place to bake in your authentication and authorisation layer.

Routes hook in to the `store` as you would expect:

```
model: function () {
  return this.store.findAll('package');
}
```

The `findAll()` method does a `return this.find(model);` for us. `find()` itself delegates to internal private methods to either fetch the records in question (from the `.json` file stored in S3), based on when the records were last fetched, or resolves them from its cached records.

When records are fetched they pass through a custom 'parse()' method. 

```
_parse: function (content) {
    var self = this, pkgRecord, repoRecord, userRecord;

    function parseNpmUser(user) {
      if (!user) {
        return null;
      }
      userRecord = {};
      userRecord.name = user.name;
      userRecord.email = user.email;
      userRecord.gravatarUrl = user.gravatar + '?s=30&d=retro';
      userRecord.profileUrl = 'https://npmjs.org/~' + user.name;
      return self.createOrUpdateRecord('npm-user', userRecord);
    }

    function parseGithubUser(user) {
      if (!user) {
        return null;
      }
      userRecord = {};
      userRecord.name = user.user;
      userRecord.profileUrl = 'https://github.com/' + user.user;
      return self.createOrUpdateRecord('github-user', userRecord);
    }

    function parseGithubRepo(repo) {
      if (!repo) {
        return null;
      }
      repoRecord = {};
      repoRecord.user = self.recordForId('github-user', repo.user);
      repoRecord.name = repo.user + '/' + repo.repo;
      repoRecord.url = 'https://github.com/' + repoRecord.name;
      repoRecord.travisBadgeUrl = 'https://travis-ci.org/' + repo.user + '/' + repo.repo + '.svg?branch=master';
      return self.createOrUpdateRecord('github-repo', repoRecord);
    }

    content.forEach(function (pkg) {
      pkgRecord = {};
      pkgRecord.name = pkg.name;
      pkgRecord.description = (pkg.description || '').trim();
      pkgRecord.createdAt = date(pkg.time.created);
      pkgRecord.modifiedAt = date(pkg.time.modified);
      pkgRecord.homePageUrl = (pkg.homepage && pkg.homepage.url) ? pkg.homepage.url : null;
      pkgRecord.keywords = (pkg.keywords) ? pkg.keywords.without('ember-data') : null;
      pkgRecord.repositoryUrl = (pkg.repository && pkg.repository.url) ? pkg.repository.url : null;
      pkgRecord.authorName = pkg.author ? pkg.author.name : pkg._npmUser.name;
      pkgRecord.owner = parseNpmUser(pkg._npmUser);
      pkgRecord.githubUser = parseGithubUser(pkg.github);
      pkgRecord.githubRepo = parseGithubRepo(pkg.github);
      pkgRecord.license = pkg.license;
      pkgRecord.bugUrl = (pkg.bugs && pkg.bugs.url) ? pkg.bugs.url : null;
      pkgRecord.starredCount = (pkg.starred) ? pkg.starred.length : null;
      pkgRecord.downloadedCount = pkg.downloads.downloads;
      pkgRecord.firstDownloadedAt = date(pkg.downloads.start);
      pkgRecord.lastDownloadedAt = date(pkg.downloads.end);
      pkgRecord.npmUrl = 'https://www.npmjs.org/package/' + pkg.name;

      self.createOrUpdateRecord('package', pkgRecord);
    });
  }
```

Pretty self-explanatory, it grabs the necessary information from the JSON payload and creates (or updates) a record based on it. What's really nice to see though is how easy it can be to do something like this. Sometimes it can seem a little murky between a basic `ic-ajax` implementation and full-blown Ember Data, but here we have a nice 'n' tidy implementation of a custom store.

On the UI side there's some sorting and filtering implemented. Whilst sorting and filtering can be considered part of your bread and butter, it's always interesting to see how something is implemented. Should you sort, filter or page client side or server side? It always comes down to the project in hand, and how many records you're dealing with. I found that parity between client and server side became much easier when the query params API landed. Which is in use here:

```
queryParams: {
    query: {as: 'q', replace: true},
    qpSort: 's',
    qpReverse: 'r'
}
```

Personally I've found by using query params and utilising the `metadata` property in your payload you can achieve most things painlessly. 

Filtering is linked via an input box, which sets a `query` property. Each item uses a separate item controller and observes for a change in `parentController.query`. Which filters down like so:

```
matchFilters: function(){
    var query = (this.get('parentController.query') || '').trim().toLowerCase();
    return !query ||
      this.get('-name').indexOf(query) >= 0 ||
      this.get('-owner').indexOf(query) >= 0 ||
      this.get('-description').indexOf(query) >= 0;
  }.property('-name', '-owner', '-description', 'parentController.query').readOnly()
```

This is then linked back to the template like so:

`{{bind-attr class='matchFilters::hidden'}}` 

Overall this was a really nice project to 'port', and I enjoyed looking through [Giovanni Collazo](https://www.twitter.com/gcollazo)'s work.

Edit: 

The Store code wasn't written by Giovanni, but [Huafu Gandon](http://www.twitter.com/huafu_g), thank you for the clarification! 

<blockquote class="twitter-tweet" lang="en"><p><a href="https://twitter.com/Kerry350">@Kerry350</a> just want to clarify that the Store code was written by <a href="https://twitter.com/huafu_g">@huafu_g</a></p>&mdash; Giovanni Collazo (@gcollazo) <a href="https://twitter.com/gcollazo/status/567446352175857664">February 16, 2015</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

*This writing has kindly been sponsored by [Pootsbook](https://twitter.com/pootsbook) and [Ember Watch](https://github.com/emberwatch).*

I want to read more articles like this:

<form accept-charset="UTF-8" action="https://formkeep.com/f/0e0fbc4cd1a7" method="POST">
  <input type="hidden" name="utf8" value="✓">
  <input type="hidden" name="article-title" value="Porting Ember Addons">
  <input type="text" name="name" placeholder="Name">
  <input type="email" name="email" placeholder="Email">
  <input type="submit" value="Submit">
</form>

 






