---
layout: post
title: "Using the 'Apply' Function in Javascript"
date: 2014-07-10 15:33:56 -0400
comments: true
categories: [javascript, apply, functions]
---

My foray into javascript has not been easy - It's meant working hard to get out of the mindset of Ruby and forcing myself into the discomfort of a relatively new language. Sort of like switching to French if you speak Spanish - there are many similarities, but relying on your knowledge of one language can actually be a debilitating crutch if you aren't carefully about what knowledge you transfer over.

I've been stuggling with understanding the `.apply` method in javascript. There isn't any really analogous function that I've found in Ruby (there probably is, but I just don't know it yet...).

One way `.apply` is used is to apply the elements in an array as arguments in a function. For example:

```js
Math.max(1,2,3,4) //=> 4

//but

Math.max([1,2,3,4]) //=> NaN
```
Instead of looping through each of the numbers in the array using `for` we can use the `apply` method to take the elements of the array and populate them into the function's arguments:

```
Math.max.apply(null, [1,2,3,4]) //=> 4
```
Javascript is clicking. Slowly.