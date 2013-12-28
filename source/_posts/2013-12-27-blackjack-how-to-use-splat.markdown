---
layout: post
title: "Blackjack? A Rubeque splat problem."
date: 2013-12-27 16:35
comments: true
categories: [Ruby, Splat, operators]
---
{% img http://img62.imageshack.us/img62/2504/midgarmeteor.jpg "Final Fantasy VII Meteor" %}

In my previous [post](https://derikulous.github.io/year/12/25/splicing-arrays/), I explained in detail how Ruby handles splicing of an array, specifically index vs. position. I've been working through Rubeque problems, and came up against a splat operator problem <b>[here](http://www.rubeque.com/problems/blackjack/solutions)</b>. I had seen the splat operator a few times in Rails, but didn't have a solid grasp on its functionality.

Before exploring the inner workings of the splat operator, and ultimately how to solve the Rubeque problem, let's review how we can add multiple arguments into a method:

``` ruby
def sum(num_one, num_two, num_three)
  num_one + num_two + num_three
end

puts sum(1, 2, 5) # => 8
```
The above example illustrates how to define a set of parameters for one method by separating each parameter with a comma. This code is easy to read, however it isn't DRY. Think about if you had a class and had to define multiple methods like this, each with multiple parameters. You can see how the above solution would get quickly obfuscated.

So, to DRY up the code, we can use the splat operator. <b>What is a splat operator?</b>
This fancy Ruby (also available in Python) operator defines a variable number of arguments (read: an unknown number of argument). It is defined with an asterisk, like so: ``` *args ```. Using the first example, we could redefine the solution as such:

``` ruby
def sum *num
  num.inject(0) { |sum, num| sum + num }
end

puts sum(1, 2, 5)
```

Going through this step by step: <br>
1) Define the sum method ``` def sum ```. <br>
2) Use the splat operator ``` *num ``` to define an argument **num**. <br>
3) Whoa, what is inject? Don't panic... inject simply combines all elements by applying a binary operation, specified by the block ``` || ```. .inject can be passed in an initial value, hence the ``` num.inject(0) ```. If we used ``` num.inject(21) ```, then we would begin summing values starting at 21. <br>
4) Define the block. According to the [Ruby docs](http://www.ruby-doc.org/core-1.9.3/Enumerable.html#method-i-inject), the block takes in an accumulator *value* and an element.<br>

What does all of this mean? In plain English, it looks like this:<br>
``` ruby
sum begins at 0
sum is defined as an array [1, 2, 5]
the first value in the array is passed to num, in this case value equals 1
sum + num => 0 + 1 => 1
sum now equals 1
the second value (2) in the array is passed to num
sum + num => 1 + 2 => 3
sum now equals 3
the last value (5) in the array is passed to num
sum + num => 3 + 5 => 8
sum now equals 8
```

The Rubeque instructions are as follows: **Write a method that takes any number of integers and returns true if they sum to 21, false otherwise. Hint: splat operator.**

The final code that we want to match is
``` ruby
assert_equal twenty_one?(3, 4, 5, 6, 3), true
```

Since we know how to use the splat operator now, we can solve the problem:
``` ruby
def twenty_one? *num
  num.inject { |sum, num| sum + num } == 21
end
```

We did not need to explicitly define the beginning value in this case. Since Ruby 1.9+, the code can be further DRYed:<br>
``` ruby
def twenty_one? *num
  num.inject(:+) == 21
end
```
I hope you enjoyed reading this post on splat operators. I learned a great deal by writing it!
















