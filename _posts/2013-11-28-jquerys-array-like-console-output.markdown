---
title: jQuery's array-like console output
date: 2013-11-28 22:51:01 
tags: code
layout: post
---
Just a quick post today. I was wondering the other day how jQuery achieves it's array-like output in console. 

For instance, when you log a jQuery instance this is the console output (example performed with `$('body')` in console):

`[<body class="editor"></body>]`

We get an array of DOM nodes, as opposed to an instance of the jQuery constructor and details of it's prototype. If, however, I assign that to a variable and then log the variable I get the expected output. So, how does this work? 

It's actually quite easy to achieve. 

jQuery mimics array-like behaviour with three things, a property called `length` on the instance, a method called `splice` and by assigning each of the elements a numbered property (`0: body.editor` and so on).
 
Lets pretend we've assigned our selector to a variable with `var thing = $('body')`. If you were to run `delete thing.__proto__.splice` and `delete thing.length` and then log `thing` again you will be returned the normal jQuery instance as you'd expect, rather than the array of number indexed properties (which are the DOM elements returned for the selector).
 
Similarly, in it's array-like form doing a `console.log()` on thing would show `__proto__: Object[0]` (notice the index), whereas when you delete the array-like parts you will see `__proto__: Object` as expected. This is due to the following in the jQuery source: 
 
```javascript
$.prototype.length == 0;
$.prototype.splice == [].splice;
```

Not overly helpful to know, but interesting nonetheless :)

