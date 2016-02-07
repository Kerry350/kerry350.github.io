---
title: Making ember-deploy adapters
date: 2015-02-26 08:36:08 
tags: 
layout: post
---
I recently published [ember-deploy-s3-index](https://github.com/Kerry350/ember-deploy-s3-index), an S3 index adapter for [ember-deploy](https://github.com/LevelbossMike/ember-deploy) ([Writeup](http://kerrygallagher.co.uk/the-making-of-ember-deploy-s3-index/)). Whilst the documentation for ember-deploy is good, I thought I'd briefly talk about some of the things I found whilst building a custom adapter, to hopefully aid anyone building their own adapters.

## Background

For context, ember-deploy is an ember-cli addon for deploying ember-cli applications. It has a nice architecture based on using different adapters for different deploy scenarios. You need to have two types of adapters, an index-adapter (which handles the `index.html` file) and an assets-adapter (which handles the other assets). Read more about it on the [ember-deploy README](https://github.com/LevelbossMike/ember-deploy).

## Telling ember-deploy an adapter exists

At the heart of it all, you'll need to add `ember-addon` to your `package.json` keywords section as always. Then in your module's main `index.js` export we define the `type` as `ember-deploy-addon`, now ember-deploy will pick up on our adapter when looping through all of the ember addons in the project, .e.g:

{% highlight javascript %}
var S3Adapter = require('./lib/s3-adapter');

module.exports = {
  name: 'ember-deploy-s3-index',
  type: 'ember-deploy-addon',

  adapters: {
    index: {
      'S3': S3Adapter
    }
  }
};
{% endhighlight %}

## Defining the Adapter

When you define your adapter it makes most sense to inherit from the base adapter class. You can import this like so: `var Adapter = require('ember-deploy/utilities/adapter');`, this uses `extend()` syntax. 

When your adapter is initially instantiated it'll be passed a number of things. `manifestSize` if one has been defined in the config, this sets the number of revisions you would like to exist before older ones are purged. A `taggingAdapter` if one has been specified in the `deploy.json` file, this would mean a custom `taggingAdapter` has been specified, all `taggingAdapter`s must specify one  method `createTag()`, which you can use to tag your revisions in-line with the users' needs. If a `taggingAdapter` isn't provided you can use the built in SHA based `taggingAdapter` by requiring it like so: `var TaggingAdapter = require('ember-deploy/utilities/tagging/sha');` and calling `createTag()`. Lastly a `ui` property is passed through that you can use to write out to the command line like so `this.ui.writeLine('A tasty message');`

## Configuring Asset URLs

Another thing you'll want to do when working with adapters like this is define a `fingerprint.prepend` property in your `Brocfile.js`. By default ember-cli will generate relative URLs, but as we'll be storing our assets somewhere else these will be pretty useless. By defining this property our asset hosts' URL will be prepended to the asset URLs within our `index.html` file.  

{% highlight javascript %}
var app = new EmberApp({
  fingerprint: {
    prepend: 'https://s3-us-west-1.amazonaws.com/my-assets-bucket/'
  }
});
{% endhighlight %}

Other than that, everything is straight forward. Use your adapter within your `deploy.json` file and away you go! 

*This writing has kindly been sponsored by [Pootsbook](https://twitter.com/pootsbook) and [Ember Watch](https://github.com/emberwatch).*

I want to read more articles like this:

<form accept-charset="UTF-8" action="https://formkeep.com/f/0e0fbc4cd1a7" method="POST">
  <input type="hidden" name="utf8" value="✓">
  <input type="hidden" name="article-title" value="Making ember-deploy adapters">
  <input type="text" name="name" placeholder="Name">
  <input type="email" name="email" placeholder="Email">
  <input type="submit" value="Submit">
</form>

