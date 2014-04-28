---
layout: post
title: "Project Euler - Pythagorean Triplets"
date: 2014-04-28 10:24:30 -0400
comments: true
categories: [project euler, math problems, ruby, pythagorean triplets]
---

I've been trying to crank through Project Euler questions over the past week or so. They are pretty challenging at first sight, but I'm finding that after looking at them for a few minutes, patterns start revealing themselves and the problem becomes much clearer. Today's question is the following:

> A Pythagorean triplet is a set of three natural numbers, a < b < c, for which,
>
> a<sup>2</sup> + b<sup>2</sup> = c<sup>2</sup>
>For example, 3<sup>2</sup> + 4<sup>2</sup> = 9 + 16 = 25 = 5<sup>2</sup>.
>
>There exists exactly one Pythagorean triplet for which a + b + c = 1000.
>
>Find the product abc

<!-- more -->

I figure that the best way to approach these problems is to break them down in to manageable chunks that I can chain together to get a final answer. In this situation, I broke the problem down like this:

1) Initialize an instance of a class with an instance variable equal to the number that is the sum of a + b + c.

2) Get all three number combinations that add up to this number.

3) Filter these combinations to those where a < b < c

4) Check available combinations for pythagorean triplet-ness

5) Find the product of the numbers in the triplet.

### The Breakdown

####Initialize an instance of a class with an instance variable equal to the number that is the sum of a + b + c.
```ruby
	def initialize(sum)
		@sum = sum
	end
```
Pretty straightforward. Set the @sum instance variable to whatever the sum is that we're given at the start of the problem (in this case, 1000).

####Get all three number combinations that add up to the sum.
```ruby
	def combinations
		combinations_adding_to_n = []
		(1..@sum).each do |a|
			(1..@sum).each do |b|
				(1..@sum).each do |c|
					combinations_adding_to_n << [a,b,c] if a+b+c == @sum
				end
			end
		end
		combinations_adding_to_n
	end
```
I'm creating a series of three nested loops, where the variable being set up for each loop is a, b, and c, respectively. Within the last loop I add [a,b,c] to the cobinations_adding_to_n array if the sum of a,b, and c is equal to the sum we've initialize the class instance with.

####Filter the combinations to those where a < b < c
```ruby
	def ordered_ascending_sum
		combinations.collect do |array_sum|
			array_sum if array_sum[0] < array_sum[1] && array_sum[1] < array_sum[2]
		end.compact
	end
```
We use the .collect method to create an array where the only items that are stored are those from combinations where a < b < c, or the the array's value at 0 index is less that its value at 1 index and its value at 1 index is less than its value 2 index. I use compact at the end of this to get rid of nil values in the array.

####Check available combinations for pythagorean triplet-ness
I built this method to check if an array [a,b,c] is a pythagorean triplet:

```ruby
	def is_pythagorean_triplet?(input_array)
		input_array[0]**2 + input_array[1]**2 == input_array[2]**2
	end
```
Test each of your possible combinations for pythagorean triplet-ness:

```ruby
	def triplet_for_n
		ordered_ascending_sum.collect do |abc|
			abc if is_pythagorean_triplet?(abc)
		end.compact.flatten
	end
```
This will provide the one array where [a,b,c] is a pythagorean triplet. On to our last step:

####Find the Product of the Numbers in the Triplet
```ruby
	def product_of_triplet
		triplet_for_n.reduce[:*]
	end
```

That's it! We get the when a+b+c = 1000, we run:

```
test = Triplet.new(1000)
puts test.product_of_triplet
```
And our answer is... Ha. You thought I'd give it away here. You'll have to run this code yourself to find out!



