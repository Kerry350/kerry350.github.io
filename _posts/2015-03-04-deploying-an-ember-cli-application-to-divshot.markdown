---
title: Deploying an ember-cli application to Divshot
date: 2015-03-04 10:06:48 
tags: 
layout: post
---
In this tutorial we're going to deploy an ember-cli application to [Divshot](https://divshot.com/). Divshot offers fully-featured Static Site hosting, backed by a CDN, CLI deployment and different environments. 

1) Head on over to [Divshot](https://divshot.com/) and click 'Sign Up'. When you're done you'll be redirected to your dashboard. 

2) We'll install Divshot's CLI tools by running `npm install -g divshot-cli`.

3) Run `divshot login` to authenticate with Divshot. You'll be redirected to a webpage, follow the on-screen instructions.

4) Create an Ember app by running `ember new <APP NAME>`.   

5) `cd` in to your project's root and run `npm install --save-dev ember-cli-divshot` to install the ember-cli Divshot addon, this is a wrapper around Divshot's CLI tools. 

6) Next run `ember generate divshot`, this will create a `divshot.json` file. You can either set the `name` property in this file equal to the name of your Divshot app, or you can just auto-generate a new app on Divshot by running the command in the next step. 

7) Deploy the app by running `ember divshot push`. This will also generate an app for you if it doesn't exist. By default the development environment will be used, but you can specify another environment like so: `ember divshot push staging`. The production environment is used for the `build` step by default, this can also be changed like so: `ember divshot push --environment=development`. 

8) You'll receive output that looks like this:

{% highlight bash %}
Application deployed to development
You can view your app at: http://<APP NAME>.divshot.io
{% endhighlight %}

Great, that's the app deployed. Feel free to navigate to that URL to see your application. You can also use a [custom domain](http://docs.divshot.com/guides/domains), but that's out of the scope of this tutorial.

*This writing has kindly been sponsored by [Pootsbook](https://twitter.com/pootsbook) and [Ember Watch](https://github.com/emberwatch).*

I want to read more articles like this:

<form accept-charset="UTF-8" action="https://formkeep.com/f/0e0fbc4cd1a7" method="POST">
  <input type="hidden" name="utf8" value="✓">
  <input type="hidden" name="article-title" value="Deploying an ember-cli application to Divshot">
  <input type="text" name="name" placeholder="Name">
  <input type="email" name="email" placeholder="Email">
  <input type="submit" value="Submit">
</form>


