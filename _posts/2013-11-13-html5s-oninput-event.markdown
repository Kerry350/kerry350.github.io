---
title: HTML5's 'oninput' event
date: 2013-11-13 21:46:00 
tags: code
layout: post
---
Hands up if you've ever had to be notified when a user has changed an `<input />`'s value. I have. Hands up if you've been doing it wrong. I have! I feel a bit stupid for not knowing about the `oninput` event, but it gives us a great way to check for user input in a consistent way. And, best part, it's well supported (including IE9 and above). 

The problem with other methods - my go to of the past being `onkeydown` or `onkeyUp` events - is that they only fire when a user keys down or up. Which sounds obvious, of course, but say a user holds a key down that event is only going to fire once despite the input changing multiple times. 

The `onchange` event suffers from much the same fate. This will only fire when the `<input />` loses focus, and that's just not all that helpful. 

`oninput` acts just as we'd expect. Telling us exactly when the input has changed. Tasty. If you must support oldIE you can always fall back to `onkeydown` / `onkeyup` events for those browsers. 

More information can be found [here](https://developer.mozilla.org/en-US/docs/Web/Reference/Events/input?redirectlocale=en-US&redirectslug=DOM%2FMozilla_event_reference%2Finput).
