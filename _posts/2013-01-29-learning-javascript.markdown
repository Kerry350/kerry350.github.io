---
title: Learning JavaScript
date: 2013-01-29 17:00:00 
tags: code
layout: post
---
This is by no means a ‘definitive’ guide or a ‘correct’ guide. The correct way to learn a language and all of it’s quirks and good parts is entirely subjective. However, there are some logical places to start, and to further progress to. 

# JavaScript 

First up, JavaScript. Learn JavaScript. Not jQuery, not MooTools, not some other library or even framework. Just the language. It’s functional, prototype based, event driven and whatever else programming -  it’s important to understand the quirks and odd parts of the language, as well as all of it’s wonderful and elegant parts. JavaScript is a lovely language to work with, and it’s future is looking even brighter with the arrival of [ECMAScript.next around the corner.](http://addyosmani.com/blog/a-few-new-things-coming-to-javascript/).

(I’ve tried to keep the resources linked to free / open-source so that everyone can dive in to and enjoy the stuff here.) 

## Resources

**Books**

There are some fantastic free books for learning JavaScript.

[Eloquent JavaScript](http://eloquentjavascript.net/)

[JavaScript Enlightenment](http://www.javascriptenlightenment.com/JavaScript_Enlightenment.pdf)

Once you’ve read one or both of those (maybe read JavaScript: The Good Parts or something else instead, who knows!), you’ll have a really good knowledge of the language as a whole.

At this point I’d recommend delving a little deeper in to the more complex parts of the language - or at least acknowledging that there’s a bit more to come back to. These are the bits that I find really important:

**Scope (variable scope, execution contexts etc) and Hoisting**

Every language has it’s own scope rules etc, it’s important to know how JavaScript’s work. Hoisting is also essential to understand (or else you might wonder why you can’t call a function, that you’re sure you should be able to etc). Seriously, hoisting is overlooked sometimes, but it’s important. 

[What you need to know about JavaScript Scope](http://coding.smashingmagazine.com/2009/08/01/what-you-need-to-know-about-javascript-scope/)

[JavaScript scoping and hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting)

[Functions and function scope](https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Functions_and_function_scope)

**Closures**  

Closures took me a while to grok (sometimes I still wonder if I **fully** understand them, especially when it comes down to memory leaks etc), and are seemingly complicated on the surface. However, they are everywhere, and a fundamental part of JavaScript development. Basically, they are a very clever way of referencing variables etc that were originally part of a different scope. Due to the way JavaScript scoping works, an inner function has access to it’s parent functions scope (and therefore all of its variables etc), say you return this inner function, it will **still** have access to things from the parent scope. That’s my pathetic explanation, these will help much more: 

[Closures from MDN](https://developer.mozilla.org/en-US/docs/JavaScript/Guide/Closures)

[Closures in JavaScript](http://james.padolsey.com/javascript/closures-in-javascript/)

[Closures: Front to Back](http://net.tutsplus.com/tutorials/javascript-ajax/closures-front-to-back/)

[JavaScript closures](http://jibbering.com/faq/notes/closures/)

[Use cases for JavaScript closures](http://msdn.microsoft.com/en-us/magazine/ff696765.aspx)

[What is a closure in JavaScript](http://davidbcalhoun.com/2011/what-is-a-closure-in-javascript)

**Anonymous functions and immediately-invoked function expressions**

Immediately-invoked function expressions are commonly used in JavaScript, they’re a nice way of creating a new scope to avoid global pollution etc. They’re all over the place, but jQuery plugins make common use of them if you’d like to see them in the wild. 

[Immediately-invoked function expression](http://benalman.com/news/2010/11/immediately-invoked-function-expression/)

[Anonyous function](http://en.wikipedia.org/wiki/Anonymous_function)

**Design Patterns**

[Learning JavaScript design patterns e-book](http://addyosmani.com/resources/essentialjsdesignpatterns/book/)

[Understanding design patterns in JavaScript](http://net.tutsplus.com/tutorials/javascript-ajax/digging-into-design-patterns-in-javascript/)

**’This’**

It’s important to understand exactly what ‘this’ is, and in what context (otherwise you can end up headbutting things when the context changes within an event handler, or something similar). But, it’s also important to understand that you can change the context of ‘this’ with things like call and apply. ‘this’ is at your mercy ;) 

[What is ‘this’ in JavaScript](http://stantona.github.com/blog/2013/01/02/what-is-this-in-javascript/)

[Better JavaScript - what is this?](http://www.jacopretorius.net/2011/06/better-javascript%E2%80%93what-is-this.html)

[Changing The Execution Context Of JavaScript Functions Using Call() And Apply()](http://www.bennadel.com/blog/2265-Changing-The-Execution-Context-Of-JavaScript-Functions-Using-Call-And-Apply-.htm)


**Prototypes and prototypal inheritance**

JavaScript isn’t a classical OOP language, it doesn’t have classes or anything like that. However, it is a prototype based language, and prototypes are beautiful once understood. Whilst you may not personally be creating your own function constructors, prototypes or instantiating anything with the ‘new’ keyword it’s still imperative to know this, in my opinion, as it’s critical to how JavaScript works ‘under the hood’ and prototypes are everywhere (array prototype, string prototype…). 

[Protoypes in JavaScript](http://net.tutsplus.com/tutorials/javascript-ajax/prototypes-in-javascript-what-you-need-to-know/)

[Understanding prototypes in JavaScript](http://yehudakatz.com/2011/08/12/understanding-prototypes-in-javascript/)

[Prototypes and Inheritance in JavaScript](http://msdn.microsoft.com/en-us/magazine/ff852808.aspx)

[Introduction to Object-Oriented JavaScript](https://developer.mozilla.org/en-US/docs/JavaScript/Introduction_to_Object-Oriented_JavaScript)

[‘OOJS’ (a random find, but an enjoyable read)](https://docs.google.com/document/d/1EDyAPOS8mYgTQAz1lJleYBO_mf_2TgzYdmqpcQCE66A/edit?hl=en_US)

**Function declarations VS Function expressions**

There is a difference (relates back to hoisting too).

[What is the difference between a function expression vs declaration in Javascript?](http://stackoverflow.com/questions/1013385/what-is-the-difference-between-a-function-expression-vs-declaration-in-javascrip)


**Memory leaks and garbage collection**

There’s a bit of a misconception that because JavaScript is a garbage collected (rather than you allocating and de-allocating memory manually) language, you need not pay attention to memory and how it works. This really isn’t true, you need to be aware that JavaScript ‘tracks’ it’s memory usage via references - when something no longer has a single reference to it, it can be cleaned up. Things like event handlers can easily cause a memory leak if not cleaned up properly. It’s important that things in your code that need them have an exit strategy. I tend to opt for a simple destroy() method, it’ll unbind necessary event handlers, null out things it needs to etc (even better when you can add these things in to a common prototype…). Be aware that traditional ‘circular references’ are handled very well by modern browsers, this should only really bother you in IE7 and below. 

[Understand memory leaks in JavaScript applications](http://www.ibm.com/developerworks/library/wa-jsmemory/index.html)

[Memory Management](https://developer.mozilla.org/en-US/docs/JavaScript/Memory_Management)

[High-Performance, Garbage-Collector-Friendly Code](http://buildnewgames.com/garbage-collector-friendly-code/)

[Managing Memory in Windows Store Apps](http://msdn.microsoft.com/en-us/magazine/jj651575.aspx)

# jQuery

jQuery is a library that makes it a breeze to work with the DOM, perform AJAX requests and all sorts of other things.

It’s nothing but good old vanilla JavaScript under the hood, don’t think you need jQuery for **everything**, use it where it aids you.  

## Resources 

**books**

[jQuery fundamentals](http://jqfundamentals.com/)

[A graphical explanation of JavaScript closures in jQuery (this is nice as it gives you an overview of closures from earlier but in a jQuery context)](http://www.bennadel.com/blog/1482-A-Graphical-Explanation-Of-Javascript-Closures-In-A-jQuery-Context.htm)

# Backbone.js

Now, once you’ve been working with JavaScript and jQuery (or whatever combination of libraries) for a while, you’ll start to crave structure, maintainability, state management and lots of other things that stop you ending up with a spaghetti code mess.

It’s not that you can’t achieve well structured, awesome code without a framework - it’s not that at all, it’s just that it’s a bit easier when you’re following tried and tested methods / patterns etc. In this case there’s a bunch of MV* frameworks at your beck and call - Backbone, Ember, Angular, Spine and so on. These give you the things you need to get the job done - models, views, controllers, routers, views etc. 

The only reason I’ve named this section Backbone.js is it’s the one I work with the most, and felt I could link to the best resources on. The others are equally great.

## Resources

**Books**

[Backbone Fundamentals](http://addyosmani.github.com/backbone-fundamentals/)

**Backbone.js memory management**

Backbone.js has recently added some really handy methods for aiding memory management - listenTo() and stopListening(). However, it’s still important to understand what’s going on with these methods, how Backbone.js handles event bindings and how things worked “in a time long ago”. They’re not magic save alls (unfortunately), you still need to know where you’re referencing things. 
 
[Backbone.js And JavaScript Garbage Collection](http://lostechies.com/derickbailey/2012/03/19/backbone-js-and-javascript-garbage-collection/)

[Zombies! RUN! (Managing Page Transitions In Backbone Apps)](http://lostechies.com/derickbailey/2011/09/15/zombies-run-managing-page-transitions-in-backbone-apps/)

Really with frameworks, I just recommend playing around. Some are very opinionated - Ember.js for example, some just give you minimal building blocks - Backbone.js. There’s no right or wrong really. Should views and models have direct references to each other? Or should they communicate purely via an Event Aggregator? Both are totally fine, it depends on what you need to build, and what it needs to do. 

I hope this helps somebody in the pursuit of learning JavaScript. And remember, JavaScript is everywhere. Fancy some server action? You’ve got Node.js, fancy writing a Windows Store App? It’s cool, you can do that with JavaScript. Also, dig right in to source code. I’ve learnt a bunch reading through code behind things like Backbone.js.
 
