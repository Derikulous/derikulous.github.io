---
layout: post
title: "How to display FontAwesome in Firefox using Rails and CloudFront"
date: 2013-11-16 16:33
comments: true
categories: [FontAwesome, CloudFront, Ruby on Rails, troubleshooting, Firefox]
---
{% img http://25.media.tumblr.com/8f8d4d311eeca74a0496233c087dbbae/tumblr_mw6av9Opmb1rbnw5do2_500.gif "5 cm per second" %}

<h3> Winter is almost upon us; the perfect time for staying indoors and coding!</h3>
<p>
  I decided to move over this blog from <a href="https://www.heroku.com/">Heroku</a> to <a href="http://pages.github.com/">GitHub pages</a>. In my next post I will go over how I did this. For now, let's focus on displaying <a href="http://fontawesome.io/"> FontAwesome</a> in Firefox using <a href="http://aws.amazon.com/cloudfront/"> AWS CloudFront.</a><br>
</p>
## Topics covered:
<ol>
<li>Cloudfront configuration</li>
<li>Cross-Origin Resource Sharing(CORS) gem</li>
</ol>
## The problem?
<p>
  Today, I was updating my [Portfolio](http://www.derikstrattan.com/) with CloudFront. However
</p>
