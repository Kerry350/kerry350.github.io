---
title: My first talk and the WYSIWYG battle
date: 2013-09-17 15:00:00 
tags: life talks
layout: post
---
Recently I've been building a WYSIWYG plugin. Plenty of these already exist, some are incredibly heavyweight such as [TimyMCE](http://www.tinymce.com/), [CKEditor](http://ckeditor.com/) and [Redactor](http://imperavi.com/redactor/). Some are lightweight, such as [Medium.js](http://jakiestfu.github.io/Medium.js/docs/) and [Zenpen](http://www.zenpen.io/) and some fit somewhere in between—[Raptor Editor](https://www.raptor-editor.com/), [wysihtml5](http://xing.github.io/wysihtml5/) etc. 

Basically, there's plenty of options. So why bother? This was just a case of nothing doing quite what I needed. This was originally needed as something for part of a spec at work; but given the sheer amount of my own time that I've put in to it, and that it may be useful to others, I've open-sourced it. I needed something that worked in a tooltip-esque fashion, rather than a fixed toolbar like most editors. Redactor has this in the form of it's 'air' mode, but Redactor is also commercial, requires a license (I'm in no way knocking this, just that it wasn't viable to be used) and the toolbar didn't look or position in the way that I needed. 

With that I took to creating something myself. I've, in all honestly, never really worked with text in depth - by this I mean things like the Range and Selection APIs. I did a little research, found out about the `contenteditable` attribute, the `.execCommand('<command>')` method, and how the Range and Selection APIs work. 

This has been a relatively fun learning experience, and the end product works quite well at the moment. There's still a lot of things to change, add, improve and bugs to fix, but it's doing okay! Despite being quite fun, it's also been genuinely anger inducing at times. Okay, **very** anger inducing. Reason being that the way `.execCommand('<command>')` applies it's formatting is totally inconsistent across browsers. In some respects this is relatively harmless, like some browsers using `<strong>` compared to `<b>`. But in other respects it's a huge difference—such as Firefox outdenting content in it's entirety, compared to Chrome unwrapping everything (text nodes being removed from their `<p>` parentNode for example), Chrome also leaves a bunch of empty `<blockquote>` elements lying around in this context—not exactly helpful!. Use of the enter key is also handled differently across browsers—Chrome opting to wrap new paragraphs with a `<div>` element, IE and Opera using `<p>` elements and Firefox opting to use `<br />` elements. The entire process was a battle against inconsistency.

Most editors seem to be split in to two camps. Some will take control of everything—like Redactor. In this case the plugin will override almost everything, so in the case of a user pressing the enter key the keydown / keyup event will be listened for and `.preventDefault()` called, from here these plugins will use selections, ranges and general DOM manipulation to achieve the desired effect. This seems simple enough until you start looking at the many, many cases you need to handle. Text being highlighted in the middle of a sentence and enter being pressed, at the end, at the beginning, selections spanning many elements-but some of them only partially and so on. 

Some of them just use the browser defaults. This was the direction I took, after first pursuing the manual root (I had to give up on this due to time constraints in the end, I may look at it again in the future). As far as I was concerned as long as the editor looked and functioned correctly to the user—that was good enough for the editing process. By this I mean there's very little difference between `<p>` or `<div>` elements to a user—they're both block level elements that can be styled the same way. However, I didn't want anyone to access the messy HTML through the public methods. So I opted for this route: browser defaults for the editing process -> `.HTML()` or `.markdown()` method is called -> 'messy' HTML is run through a `.sanitise()` method -> the sanitised HTML is handed off to the HTML to Markdown parser -> sanitised HTML or Markdown is returned.

Depending on the complexity of the formatting options you need, I can see why this wouldn't work in many respects. However, as I was only looking to allow bold, italic, link, unordered list, ordered list, undo, redo, heading and blockquote functionality it worked out well, well, despite my blockquote code being a bit of a travesty!

I didn't intend for this to be an in-depth writeup of my experience building the plugin though so I'll stop now—but if anyone would be interested in such a writeup let me know.

So this leads me on to giving my first ever talk. @pootsbook had contacted me to ask if I would like to speak at @jsnortheast. Now, for those who don't know, I suffer from very bad anxiety issues—medication, CBT therapy, self-help books—I do / have done the whole shebang. Not surprisingly my first inner reaction was "no", but I knew deep down that if I could give a talk it would be a massive step for me. I'd always explained public speaking was an 11/10 for me to my therapist.

Turns out I gave my talk, made it to the end and didn't die. And the biggest privilege of all was that I got to give it in front of my Dad (this wasn't planned, just really convenient). Whilst it was only a crowd of around 15, that's a huge step for someone like me. I received some really great feedback—and that was such an awesome feeling!

The slides from the talk are [here](https://github.com/Kerry350/WYSIWYG-presentation). I'll also be re-giving the talk on the 30th September at [SuperMondays](http://www.supermondays.org/2013/09/16/supermondays-lighting-talks-2013/). 

All in all I'm trying to put myself out there a bit more nowadays. As someone who has anxiety issues and constantly feels the tensions of imposter syndrome, this can be quite hard, but hopefully by giving talks and getting code opened up publicly on Github this may improve. Or, you know, do the exact opposite :) 




