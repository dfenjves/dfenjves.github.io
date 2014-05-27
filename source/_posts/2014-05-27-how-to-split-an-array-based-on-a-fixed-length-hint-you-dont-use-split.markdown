---
layout: post
title: "How to split an array based on a fixed length (hint: you don't use .split)"
date: 2014-05-27 11:36:42 -0400
comments: true
categories: [arrays, ruby, split, scan, fixed length]
---
Say you have a string that you need to split up based on a fixed length of characters (in this case, every five: `"jamesjonesspiesrabidbirds"`. Unfortunately, the split method doesn't help us here. 

Instead, we can use `.scan`. This method looks for regex matches and adds them to an array, rather than taking a string and breaking it up based on a delimiter. It's a useful method in those cases where .split just can't handle the job.

```ruby
	"jamesjonesspiesrabidbirds".scan(/.{5}/) #=> ["james", "jones", "spies", "rabid", "birds"]
```

The regex `/.{5}/` stands for "exactly 5 of any character" - the . means 'any character', and the curly braces with one number means "exactly this amount". The scan method adds every five characters of the string to the array it returns, to give us ["james", "jones", "spies", "rabid", "birds"].
