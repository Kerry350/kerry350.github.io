---
title: Deploying an ember-cli application to Firebase
date: 2015-03-02 11:29:48 
tags: 
layout: post
---
In this tutorial we're going to deploy an ember-cli application to Firebase. [Firebase](https://www.firebase.com/) is a Backend as a Service provider that makes storing and using realtime data easy. They have libraries for all of the popular web and mobile platforms, failing that you can always fall back to the REST API too. We'll be using [`ember-cli-firebase-hosting`](https://github.com/dhaulagiri/ember-cli-firebase-hosting) in this tutorial, which is a wrapper around the official `firebase-tools` package. 

1) Head on over to [Firebase](https://www.firebase.com/) and [sign up](https://www.firebase.com/signup/) (free for the 'hacker' plan). You'll then be redirected to your Dashboard. 

2) An app, 'My First App', will be sat waiting for you. Feel free to customise the settings on this app. If you've already developed your app using Firebase you'll have all of this setup already. 

3) Let's make an Ember app, with `ember new <APP NAME>`.

4) First we'll install the official Firebase command line tools, run `npm install -g firebase-tools`, which gives us access to the `firebase` command.

5) `cd` in to the root of your project and run `firebase login` to authenticate with Firebase, follow the prompts.

6) Run `npm install --save-dev ember-cli-firebase-hosting` to install the ember-cli [Firebase Hosting](https://www.firebase.com/docs/hosting/quickstart.html) addon.

7) Run `ember generate firebase-hosting`, this will create a `firebase.json` file for you. Running this is the equivalent of running `firebase init`. If the app name in your Firebase dashboard doesn't match your Ember app's name, go ahead and change the `firebase` property in the `firebase.json` file. If in doubt about your app's name just run `firebase list`.   

8) Now we run `ember firebase deploy` to deploy the app.

9) You'll receive some output that looks like this:

{% highlight bash %}
Successfully deployed

Site URL: https://<APP NAME>.firebaseapp.com, or use firebase open
{% endhighlight %}

Sweet, that's the app deployed. Feel free to navigate to that URL to see your application. You can also use a [custom domain](https://www.firebase.com/docs/hosting/guide/custom-domain.html), but that's out of the scope of this tutorial. 

*This writing has kindly been sponsored by [Pootsbook](https://twitter.com/pootsbook) and [Ember Watch](https://github.com/emberwatch).*

I want to read more articles like this:

<form accept-charset="UTF-8" action="https://formkeep.com/f/0e0fbc4cd1a7" method="POST">
  <input type="hidden" name="utf8" value="✓">
  <input type="hidden" name="article-title" value="Deploying an ember-cli application to Firebase">
  <input type="text" name="name" placeholder="Name">
  <input type="email" name="email" placeholder="Email">
  <input type="submit" value="Submit">
</form>
