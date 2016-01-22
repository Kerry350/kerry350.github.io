---
title: Deploying an ember-cli application to Heroku
date: 2015-02-13 10:09:23 
tags: 
layout: post
---
In all honesty this post feels like a little bit of a cheat due to the great work by [Tony Coconate](https://github.com/tonycoco). I recently needed to deploy an Ember CLI app to Heroku. Normally — in the land of Digital Ocean, Amazon EC2 instances et al — I'd install and configure Nginx to serve the static files. 

Life should be quick 'n' easy with a PaaS though, and it is. We can utilise one of [Heroku's Buildpacks](https://devcenter.heroku.com/articles/buildpacks). Buildpacks allow us to tell Heroku's slug compiler how to execute our code: "At the heart of the slug compiler is a collection of scripts called a buildpack". 

In this particular case Tony has put together an [Ember CLI Buildpack](https://github.com/tonycoco/heroku-buildpack-ember-cli) that handles the installation of Node, Nginx and generates a production build of your app. There's also a bunch of really handy options for enabling HTTPS / SSL, handling authentication and so on. So let's get an app deployed:

1) Head on over to [Heroku](https://www.heroku.com) and sign up for free.
2) Next we'll install the [Heroku Toolbelt](https://toolbelt.heroku.com/), this gives us access to all of Heroku's command line goodness. 
3) Once you've installed the Toolbelt you'll want to authenticate with Heroku, run the following: `heroku login` and follow the prompts. 
4) Now we're ready to create our app Heroku side, run the following (in your app's directory, and make sure you have a Github repo setup here for your app): `heroku create --buildpack https://github.com/tonycoco/heroku-buildpack-ember-cli.git`
5) When you're ready to deploy your app you just need to run `git push heroku master`. And that's it!

## What does it do?

The Buildpack itself does a number of things for us (sometimes more, sometimes less depending on which options we've specified). 

During the setup process Node.js is installed from Heroku's S3 mirror, Nginx is installed, configs are copied in to the relevant locations, PATH variables are defined, `node_modules` and `bower_components` folders are handled (i.e. handling whether they may or may not have been checked in to source control), dependencies are installed (via npm and Bower) etc. Phew, and breathe! 

After the initial setup our application is built like so `node_modules/ember-cli/bin/ember build --environment $build_env`, just as we would build it locally. The `/dist` directory is then served via Nginx. There is an exhaustive configuration file that [looks like this](https://github.com/tonycoco/heroku-buildpack-ember-cli/blob/master/config/nginx.conf.erb). 

*This writing has kindly been sponsored by [Pootsbook](https://twitter.com/pootsbook) and [Ember Watch](https://github.com/emberwatch).*

I want to read more articles like this:

<form accept-charset="UTF-8" action="https://formkeep.com/f/0e0fbc4cd1a7" method="POST">
  <input type="hidden" name="utf8" value="✓">
  <input type="hidden" name="article-title" value="Deploying an ember-cli application to Heroku">
  <input type="text" name="name" placeholder="Name">
  <input type="email" name="email" placeholder="Email">
  <input type="submit" value="Submit">
</form>

 

