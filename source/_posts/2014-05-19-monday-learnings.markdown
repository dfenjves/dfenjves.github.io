---
layout: post
title: "Monday Learnings"
date: 2014-05-19 16:57:27 -0400
comments: true
categories: [pry, ]
---

I learned some cool things today, which I'd like to share here.

1) The `each_cons` method. It's great. While working on [Project Euler #11](https://projecteuler.net/problem=11) with a friend, he showed me this method. It is applied to an array and takes an argument for n elements - it then returns an enumerator with the different variations of 3 consecutive elements from the array. I'm not sure I'm doing it justice, so here is an example:

```ruby
	array = [1,2,3,4,5,6,7,8]
	array.each_cons(3).to_a 3 # => [[1, 2, 3], [2, 3, 4], [3, 4, 5], [4, 5, 6], [5, 6, 7], [6, 7, 8]]
	
```
This was very helpful in finding the products of each possible set of four consecutive elements (horizontally) in the problem's grid. Nice.
<!-- more -->

2)Editing a Method in Pry.
Very helpful. Say you've defined a method in pry:

```
def hello
	puts "Hello"
end
```
If you type `edit hello` in pry after this, it will open up a text editor for you to modify the method. As soon as you close and save the document, it will reload the method in pry for you to play with.


3) Using the ampersand to turn a method symbol into a proc. Check it out:
```ruby
["head", "shoulders", "knees", "toes"].collect {|word| word.reverse}
#=> ["daeh", "sredluohs", "seenk", "seot"]
```

Is the same as doing:
```
["head", "shoulders", "knees", "toes"].collect(&:reverse)
#=> ["daeh", "sredluohs", "seenk", "seot"]
```
This is because the ampersand converts a symbol object to a proc for use with enumerable methods. 

Small wins. That's what it's all about.
