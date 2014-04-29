---
layout: post
title: "Optimus Prime Optimizes Prime"
date: 2014-04-29 10:04:04 -0400
comments: true
categories: [prime numbers, project euler, optimization]
---
-> ![Optimus Prime](/images/Optimus.jpg) <-

Hi there folks. Optimus Prime here. Few of you know this, but when I'm not out blowing up bad guys with my ion-blaster and swinging my energon-axe, I enjoy solving Project Euler problems over a bowl of oatmeal and a cup of coffee at breakfast. Weird, right? It's like seeing your teacher walking out on the street, or bumping in to your shrink at the grocery store. Anyway...

I wanted to talk to you about a piece of a Euler problem that I particularly enjoyed optimizing (the problem itself is [here](https://projecteuler.net/problem=10)). It's the method I call `is_prime?`, which, as it's name suggests, checks to see if a given number is prime. Sure, I could use the built in ruby method `Prime.prime?(n)`, but where would the fun be in that?

<!-- more -->

Here's my first attempt at defining this method. We're essentially brute-forcing our way through every possible number, returning `true` unless one of those numbers ends up dividing without a remainder into `n`:

```ruby
	def is_prime?(num)
		prime_result = true
		(2..num-1).each do |n|
			if num % n == 0
				prime_result = false
			end
		end
		prime_result
	end
```
The optimizations I've come up with all have to do with finding ways other than brute force to test the prime-ness of the number. To start, testing for factorization of numbers above one half of a given `n` is useless. i.e. 32 can never have a factor (other than itself) above 16. So let's change the range to one half of the given number:

```ruby
	def is_prime?(num)
		prime_result = true
		(2..num/2).each do |n| # optimization no.1
			if num % n == 0
				prime_result = false
			end
		end
		prime_result
	end
```

Ok, so if we're checking to see if the number 997 is prime (it is) we've now gone from looping 996 times to looping 497 times. A good start.

But if our `num` is divisible by 2, why are we still checking divisibility by 4,6,8 and every other even number up to num/2 ? That's about as silly as Megatron thinking he could win the [battle of Autobot city](http://tfwiki.net/wiki/Battle_of_Autobot_City) by ordering the Constructicons to merge into the gestalt Devastator. Sheesh.

What to do about this? My solution is to add a `break` in the loop if num % n == 0. Once we find that a number is not prime, we can end the loop. It's redundant to have to continue checking:

```ruby
	def is_prime?(num)
		prime_result = true
		(2..num/2).each do |n| # optimization no.1
			if num % n == 0
				prime_result = false
				break # optimization no.2
			end
		end
		prime_result
	end
```

Now, if we're checking to see if the number 1000 is prime, we've gone from 999 loops, to 499 loops, to <b>1 LOOP</b>. We break out of the loop because we see that 1000 % 2 == 0. Amazing.

Our last optimization is one that isn't so straightforward. In fact, the only reason I know about this one is that I hang around transformers that are way smarter than I am. Here it is: You never have to check for prime factors above the square root of the number you're looking for. For example, the number 36 will not have any prime factors about its square root, 6. Since 2,3,4 and 6 all factor in to 36 and are less than or equal to it's square root, 36 is not prime. So if nothing at or under the square root of a number is a factor of the number, then that number has to be prime. We replace optimization #1 with our new optimization here:

```ruby
	def is_prime?(num)
		prime_result = true
		(2..Math.sqrt(num)).each do |n| # optimization no.3
			if num % n == 0
				prime_result = false
				break # optimization no.2
			end
		end
		prime_result
	end
```

Here is a breakdown of iterations for each of our `is_prime?` methods:

<table class='table table-hover'>
	<tr>
		<th>Number to test</th>
		<th>Un-optimized</th>
		<th>Optimization 1</th>
		<th>Optimization 2</th>
		<th>Optimization 3</th>
	</tr>
		<td>20</td>
		<td>19</td>
		<td>9</td>
		<td>1</td>
		<td>1</td>
	<tr>
	</tr>
		<td>997</td>
		<td>995</td>
		<td>497</td>
		<td>497</td>
		<td>30</td>
	<tr>
	</tr>
		<td>1000</td>
		<td>998</td>
		<td>499</td>
		<td>1</td>
		<td>1</td>
	<tr>
	</tr>
		<td>5915587277</td>
		<td>5915587275</td>
		<td>2957793638</td>
		<td>2957793638</td>
		<td>76911</td>
	<tr>
</table>


Cool, eh? That's all I have for today. Time to get back to battling the Decepticons.