---
layout: post
title: "Splicing arrays"
date: 2013-12-25 19:25
comments: true
categories: [Ruby, Splicing]
---
{% img https://encrypted-tbn3.gstatic.com/images?q=tbn:ANd9GcTyxXhXqKTH4NUHw7xq9TYKJ_pSt859FONRpsndwnub4fFW-7Cj "Ghost in the Shell" %}

Merry Christmas and happy holidays. Lately, I have been digging deeper to understand core Ruby principles away from web development. One of the tools that I am using are the  [Ruby Koans](http://rubykoans.com/) - a set of fun, challenging problems that build on each other as you progress through levels. As such, solving these problems is a great way to learn about Ruby syntax, and under-the-hood Rubyisms.<br>

I was working through a section in the Koans called "about_arrays", specifically test_slicing_arays.

For example:
``` ruby about_arrays.rb
def test_slicing_arrays
  array = [:peanut, :butter, :and, :jelly]

  assert_equal __, array[0,1]
  assert_equal __, array[0,2]
  assert_equal __, array[2,2]
  assert_equal __, array[2,20]
  assert_equal __, array[4,0]
  assert_equal __, array[4,100]
  assert_equal __, array[5,0]
end
```
<br>
In this example, an array is defined and includes four symbols; <i>peanut</i>, <i>butter</i>, <i>and</i>, <i>jelly</i>. There are a number of assertions to be made. Where the underscore exists, the user submits their answer.
For exampl:
``` ruby about_arrays.rb
def test_slicing_arrays
  array = [:peanut, :butter, :and, :jelly]

  assert_equal [:peanut], array[0,1]
  assert_equal [:peanut, :butter], array[0,2]
  assert_equal [:and, :jelly], array[2,2]
  assert_equal [:and, :jelly], array[2,20]
  assert_equal [], array[4,0]
  assert_equal [], array[4,100]
  assert_equal nil, array[5,0]
end
```
<br>
The first argument accepts the <b>"lower bound"</b>, while the second argument is the <b>"range of elements"</b> that you want to pull out of the array, beginning with the <b>"lower bound"</b>. So going through the list, I understood up until the last line: ``` ruby assert_equal nil, array[5,0]```.

So, really, I didn't understand anything. After a lot of stackoverflow searching, I came across this [post](http://stackoverflow.com/questions/3568222/array-slicing-in-ruby-looking-for-explanation-for-illogical-behaviour-taken-fr?rq=1).

What is exactly happening here?

In my head, how I thought indices and arrays worked was this:
``` ruby
  :peanut   :butter   :and   :jelly
     0         1        2       3
```
Following from this logic, then ``` [], array [4,0] ``` also wouldn't make sense, yet that is the solution. *Swoon*

According to the way splicing works, the indices are defined more like the following:

``` ruby
  :peanut   :butter   :and   :jelly
0         1         2      3        4
```
<br>
It's now worth noting that finding the range of an array is different than finding index.
When an array is spliced, as in ``` array[4,0] ```, the splice does not identify the elements themselves, but places **BETWEEN** the elements. This is a key distinction from ``` array[4] ```; here, Ruby is pointing at index 4. This would result in nil. Try it for yourself!

**Simply put: slicing and indexing are different operations, and as such, include different operations, and shouldn't be directly compared.**

