---
title: The making of ember-deploy-s3-index
date: 2015-02-20 15:24:01 
tags: 
layout: post
---
This week I published my first (public) npm module: [ember-deploy-s3-index](https://www.npmjs.com/package/ember-deploy-s3-index) which is an index adapter for [ember-deploy](https://github.com/LevelbossMike/ember-deploy).  

There seems to be two front-runners in the ember-cli deployment world at the moment: [ember-cli-deploy](https://github.com/achambers/ember-cli-deploy) and [ember-deploy](https://github.com/LevelbossMike/ember-deploy). ember-cli-deploy has been around longer, but I decided to build an adapter for ember-deploy. They're both fantastic projects built on the premise of 'Lightning Fast Deploys'. I chose ember-deploy because it seemed to have more participation in the creation of custom adapters, a [custom Azure index and assets adapter](https://github.com/duizendnegen/ember-deploy-azure) had already been contributed, and the default [S3 assets adapter](https://github.com/LevelbossMike/ember-deploy-s3) already existed.   

The architecture of both addons buys in to the concept of ['Lightning Fast Deploys'](https://www.youtube.com/watch?v=QZVYP3cPcWQ), which was a talk originally given at RailsConf. The idea is that your assets and static files are de-coupled from your backend code. Back in the day changing your front-end code meant waiting for your backend server to restart or compile assets or any other slew of operations — and it made no sense. Just as we should decouple web apps from their data source, the build pipeline and deployment strategy should be different, and catered, too. By default both addons work on the premise of placing your `index.html` file revisions in a key-value store such as Redis (your index adapter), and your assets in some form of static storage (your assets adapter). By storing these revisions you have a great deal of flexibility. You can roll-back boo-boos and preview a revision of your web app without actually making it live to the world. Each revision will link to it's own "snapshotted" and fingerprinted set of assets.   

ember-deploy-s3-index deviates from this idea slightly in that the `index.html` revisions are also placed in to static storage, rather than a key-value store. The default S3 assets adapter does upload your `index.html` file for you, so if you were just looking for something to upload the base `index.html` file for you, this would be overkill. This adapter aimed to deliver the entire revision and preview philosophy to S3.

I figured with this approach the adapter could use one set of code, but the end user could use the files in two ways, either serve the `index.html` straight from S3, or use it as storage for a proxy on a backend server.

This is quoted from the repository README, but explains the two scenarios in-depth:

"The idea with this adapter is you may want to serve your index.html file directly from a bucket, or you may have a backend server setup. Technically the default assets adapter does upload your index.html file, but this adapter works on the premise of having a bucket(s) dedicated to your index files, so that you can take advantage of the revisioning and previewing capabilities.

The adapter leaves two methods of handling the index files open to you (implementation comes down to you). With direct serving the idea is you have set your index bucket up as a 'static site' and when people navigate to your website they're hitting the bucket directly, and thus get served the index.html file contained within it. This is great if your Ember app relies on 3rd Party APIs, CORS or maybe doesn't communicate with an API at all (games etc). You still have revisioning capabilities here for rollbacks etc, and previewing capabilities by using one extra bucket.

proxy serving assumes you have a backend server setup of some kind, and are using S3 as a storage mechanism rather than the host itself. Using this mode you would query your S3 bucket directly, and serve the results, just like lookups with the default Redis adapter. On application boot you could query for the current implementation which would be that represented by index.html, store this in memory, and serve on consequent requests. If a revision was requested via a query parameter you can query the bucket for the revision <app-name>:<sha>.html and serve what's returned."

What I loved about this was that the techniques could be used for applications that didn't care for, or necessarily have, a backend server. You could have revisioning and previewing capabilities on an Ember based game, for example.   

Despite working with node.js for almost two years now (one year in a professional capacity), I've never actually published a module. I'm sure that makes me some sort of anomaly. I've worked on plenty of internal / private modules though, and it's always nice 'n' easy. One thing I'd recommend if you're developing modules locally is `npm link`, this allows project's to use your local (unpublished) modules. 

{% highlight bash %}
cd <project dir>
npm link <module dir>
{% endhighlight %}

And this [sets up a set of symlinks for you](https://docs.npmjs.com/cli/link). 

In my projects `package.json` I've always added: `"<module name>":"latest"` to use these local modules (there may be a nicer way of specifying them). 

Turns out publishing to npm is easy too. 

{% highlight bash %}
cd <module dir>
npm publish ./
{% endhighlight %}

I did need to run `npm adduser` and follow the prompts first, this just makes npm [aware of you as a user](https://docs.npmjs.com/cli/adduser).

And with that, my first npm module is out there. I hope it's useful, and of course thanks to [Michael Klein](https://github.com/LevelbossMike) for ember-deploy itself. If you'd like to see a detailed walkthrough of how to setup your S3 buckets and actually deploy an Ember app to S3, with revisioning and previewing abilities, there's an article [here](http://kerrygallagher.co.uk/deploying-an-ember-cli-application-to-amazon-s3/).  

*This writing has kindly been sponsored by [Pootsbook](https://twitter.com/pootsbook) and [Ember Watch](https://github.com/emberwatch).*

I want to read more articles like this:

<form accept-charset="UTF-8" action="https://formkeep.com/f/0e0fbc4cd1a7" method="POST">
  <input type="hidden" name="utf8" value="✓">
  <input type="hidden" name="article-title" value="The making of ember-deploy-s3-index">
  <input type="text" name="name" placeholder="Name">
  <input type="email" name="email" placeholder="Email">
  <input type="submit" value="Submit">
</form>

 
