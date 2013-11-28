---
layout: post
title: "Hello, world"
date: 2013-10-28 19:51
comments: true
categories: [Rake error, Hello]
---
Hello, world.

Octopress goodness. In true coding fashion, I had a bit of trouble creating my first post thanks to an error with Rake.

Specifically, when I typed the command $ rake new_post["Hello world"], I was greeted with the error: **You have already activated rake 10.1.0, but your Gemfile requires rake 0.9.2.2. Using bundle exec may solve this.**

After a bit of sleuthing around, I tried a few scenarios to fix this error, namely [here](http://www.flowerchild.org.uk/archive/2013/07/19/octopress-rake-errors-you-have-already-activated-x-but-your-gemfile-requires-y.html). However, this work around required that I **USE** rake. /facepalm <br>
There's a better fix! In the gemfile, I deleted '~> 0.9' from the **Rake** gem. Then I went into the gemfile.lock file, searched for Rake, and changed the number 0.9.22 to **10.1.0**. Voila, rake now works!
