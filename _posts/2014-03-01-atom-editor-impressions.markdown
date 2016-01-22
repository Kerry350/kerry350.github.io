---
title: Atom editor impressions
date: 2014-03-01 16:09:18 
tags: 
layout: post
---
## First Impressions
I was lucky to be provided an invite for Github's new [Atom](http://atom.io) editor by @pootsbook, who was hooked up by @isamlambert who does clever things with databases over at Github. 

I thought I'd note down some of my first impressions; given it's beta status and burst on to the scene I think people will be looking to see what it's like. 

So, on first impression Atom is an awful lot like Sublime Text. The UI is practically identical, shortcuts match and there's even similarities down to Atom having a Command Palette (the shortcut for that is also exactly the same). In some cases Atom actually has two shortcut mappings - it's own and then one to match Sublime. For me this isn't actually a complaint, if I can learn one set of transferable shortcuts I'm all for that. But then things start to divert.

## Navigating files

Atom has a fuzzy finder just like Sublime. You can use `cmd-t ` to quickly locate matching files. `cmd-b` will let you search through open files, and even cooler (if you're using Git) is `cmd-shift-b` which will search the list of files modified and untracked in your project's repository. 

![Fuzzy Finder](/assets/images/2014/Mar/Screenshot_2014_03_01_10_57_43.png)

## Symbol navigation

`cmd-r` will open up a list of all symbols (such as method definitions), you can also fuzzy filter these just like with files. 

![Symbol Navigator](/assets/images/2014/Mar/Screenshot_2014_03_01_10_58_06.png)

## Atom on the command line

I actually really liked this. In terminal you can `atom file.js` to open any file in Atom. Whilst this is obviously available in Sublime Text too it did involve some manual intervention. You'll also have access to the Atom Package Manager, running `apm install <package_name>` for instance will install an Atom package for you. Of course you're more than welcome to install said packages from the GUI too.  

## Git functionality 

Coming from Github I naturally expected Atom to have good Git integration - and it does. In the bottom right you have information regarding your current branch and how many commits have been made. In the line number gutter a pleasant little orange mark is added to show you which code is new since the last commit. There's also shortcuts for easily navigating between diffs. If you pop up to the Git package menu you also have access to things like Git blame. 

![Git gutter markers](/assets/images/2014/Mar/Screenshot_2014_03_01_11_00_56.png)

## Markdown preview 

If you're busy editing a Markdown file you can use the shortcut `ctrl-shift-m` to see a preview of the rendered HTML. I really liked this. 

![Markdown preview](/assets/images/2014/Mar/Screenshot_2014_03_01_11_03_15.png)

## Technology 

Whilst Atom is a desktop application, it's built around 'web' technology. If you wish to style Atom you simply use LESS. If you wish to build a package or extend Atom in some way you use JavaScript. Being based on LESS this will inevitably make creating themes and syntax highlighting packages easier. In the develop menu you even have access to an inspector - no different from Chrome's Web Inspector.

![Dev Tools](/assets/images/2014/Mar/Screenshot_2014_03_01_11_04_40.png)

## Settings 

This was one thing where I really did think "yeah, this tops Sublime". Atom has a GUI for changing settings. Not only can you change settings, you can also browse and install themes and packages from here.

![Settings panel](/assets/images/2014/Mar/Screenshot_2014_03_01_11_05_12.png)

## TODO!

This was one of those subtle additions that I really liked. I often leave TODO comments in my code - in fact, lots of developers do. It's an easy way to note down what needs looking at in the future. In Atom the 'TODO' part of the comment is highlighted in blue.

![TODO comments](/assets/images/2014/Mar/Screenshot_2014_03_01_12_30_44.png)

## Conclusion

Atom looks really promising and I'm enjoying using it. Github aren't lying when they say that Atom is extendable down to it's core. The technologies Atom is built around provide developers an easy way to get stuck in. Packages also seem to be getting converted over very rapidly. I was hardly missing any packages that I had in Sublime after a couple of searches. 












