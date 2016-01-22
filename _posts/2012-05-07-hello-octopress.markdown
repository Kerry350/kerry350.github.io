---
title: Hello Octopress
date: 2012-05-07 15:00:00 
tags: code
layout: post
---
So, I’ve really wanted to get back in to blogging. I’ve been neglecting my writing itch for a while. I have my ‘regular’ blog over at Tumblr, sure, but that’s for cute rat pictures and general tid-bits. This space is where I want to write about web related topics. My personal site is always sorely neglected by me, usually due to time (and laziness, of course). 

In previous carnations it’s used WordPress, it was last using ExpressionEngine (which I’ve always been fond of). However, I wanted something I could get up and running quickly, very quickly, this time. No starting from scratch with stylesheets, templates, databases, CMS installation etc. Not because I don’t enjoy hand crafting a site from the ground up, but because I realised it just wasn’t going to happen…anytime soon.

Enter Octopress. 

Octopress is an insanely awesome framework for Jekyll, that’s Jekyll the static site generator that powers Github Pages.

I decided to set up Octopress on my own hosting; instructions are available for deployment to Github Pages, Heroku or via Rsync.

My hosting is provided by Webfaction, who to be quite honest are incredible. There’s no affiliate links or anything like that here, I just really appreciate them as a solid host, offering *a lot* of features.

The steps to setting up Octopress on Webfaction were easy-peasy. 

First, I installed Octopress itself following [the official guide](http://octopress.org/docs/setup/).

That’s our blog set up locally. 

I then decided I would use [Rsync for deployment](http://octopress.org/docs/deploying/rsync/).

Then it was just a case of configuring my Rakefile using the settings found in the deployment section of [this](http://www.viggiosoft.com/blog/blog/2011/09/28/setting-up-a-blog-with-octopress/) well written guide.

If you’ve found your way here looking for Webfaction and Octopress advice, the only thing the above guide really misses out is how to add your SSH key to Webfaction’s authorised keys list. It’s okay though, all of that information is [here](http://docs.webfaction.com/user-guide/access.html) in the official Webfaction guide.

And that was that, I ran the rake commands as per the Octopress instructions and everything was live and working. Simples. 

(At this point you will want to go and create a repository somewhere though, and commit your blog’s source there).

Everything about Octopress is…well, just swell for a geek. You write your posts using Markdown. If you want to insert code snippets, [that’s not a problem](http://octopress.org/docs/blogging/code/). Templates are fully customisable, but a lovely HTML5 theme comes as standard. Deployment is painfully simple. 

The hope is that this will get me writing again :)
