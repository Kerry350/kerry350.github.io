---
title: Common JavaScript gotchas
date: 2014-05-01 17:47:28 
tags: 
layout: post
---
This topic has been done to death, but I've seen all of these lurking around in production codebases. I've found these are really common gotchas, but also dangerous ones, and evidently they're making their way out in to the wild. Lets help kill them with fire!

## Objects on a prototype

In JavaScript technically everything is passed by value. Primitive values (string, number, boolean etc) are straight up passed by value. Objects are passed by reference, but the reference itself is passed by value. The key thing to understand is that if two variables reference the same object, changing the object in one place will cascade through to the second. 

So why does this matter? Because it's a source of confusion for many starting out with JavaScript.

Imagine that you have a User prototype.

{% highlight javascript %}
function User() {
	this.name = 'Kerry';
};

User.prototype = {
	setName: function(name) {
    	this.name = name;
    },
    
    config: {
    	level: 1, 
        flag: 3
    }
};
{% endhighlight %}

Then we make two instances of this strange Kerry User. 

{% highlight javascript %}
var userOne = new User();
var userTwo = new User();
{% endhighlight %}

We then change the level on `userOne`'s config.

{% highlight javascript %}
userOne.config.level = 7;
{% endhighlight %}

Now we'll check the output of logging both `userOne` and `userTwo`'s config. 

{% highlight javascript %}
console.log(userOne.config, userTwo.config);
Object {level: 7, flag: 3} Object {level: 7, flag: 3} 
{% endhighlight %}

They both have a level of 7, despite the fact we only changed it on one. This is because of the object being passed by reference. 

To circumvent this all we need to do is assign this config object in the constructor. 

{% highlight javascript %}
function User() {
	this.name = 'Kerry';
    
    this.config = {
    	level: 1, 
        flag: 3
    };
};
{% endhighlight %}

Now each instance gets it's own reference to a config object. Of course there's nothing stopping you adding an object on the prototype that you 'clone' from or use `Object.create()` on.

I found questions around this topic often arose in the Backbone.js community, same principle, but instead of assigning things in the constructor use the `init` method.

## Using `for in` on Arrays

`for in` is not to be used on Arrays. Just don't do it. `for in` should be used for iterating over the properties in an object. To loop through an array you should use `forEach()`, or if the situation suits something like `map()` or `filter()`, but for just straight up looping `forEach()` is the best option alongside a traditional `for` loop.

Things may seem to work fine at first but there's a couple of issues here. 1) Order is not guaranteed. 2) All properties will be enumerated, including properties you've added on the prototype.

## Misunderstanding `bind()`

In JavaScript we can change the context of `this` with `call()` and `apply()`, old news right? But something else we can use is `bind()`. `bind()` is wonderful, it allows us to create a **new** function permanently bound to the context we specify. The key here is that a brand new function will be returned. Given this example: 

{% highlight javascript %}
var someObj = {
  bar: 'lemon'
};

function foo() {
	this.bar = 'baz';
};

var handler = foo.bind(someObj);
{% endhighlight %}

The `handler` function and the `foo` function will not be equal, they are completely different functions. The reason this generally slips people up is with event handlers - which is also where `.bind()` happens to be super useful. You may innocently add an event listener like so `el.addEventListener('click', foo.bind(someObj));` and then try to unbind it later like so `el.removeEventListener('click', foo);`, this won't work. You should instead store a reference to `handler` and use that to both add and remove the events. 

It's important to keep a good eye on your events, if events stick around when they're not supposed to it's an easy way to run in to memory leaks and strange behaviour. 

And that's it, three little gotchas that I see quite a bit, but they're all easy to remedy :)









