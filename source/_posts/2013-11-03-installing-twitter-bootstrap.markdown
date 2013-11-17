---
layout: post
title: "Installing Twitter Bootstrap"
date: 2013-11-03 12:34
comments: true
categories: [Twitter Bootstrap, installation, troubleshooting, front-end]
---
# Mission: Install and use Twitter Bootstrap with Ruby on Rails
## **Boss**: Twitter Bootstrap


I remember when I first started using [Twitter Bootstrap](http://getbootstrap.com/) in early Sept 2013. I really had no idea what I was doing, and spent countless hours ineffectively using the CSS files. As a result, I found myself very frustrated and wondering why this plugin was any better than just creating CSS from scratch. While participating in Startup Weekend, a friend showed me what I was doing wrong and my whole approach to using Twitter Bootstrap changed. I dove into the documentation and found a number of inspiring CSS/Javascript effects. In an effort to help others learn from my mistakes, I've created the following tutorial to get you started on basic installation and usage in your Ruby on Rails project.

## **Installation**

Installing Twitter Bootstrap in your rails project is mostly as simple as requiring it in your gemfile. However, there are a few flavors that you can install, as well as two versions (2.3.2 or 3.0.1).

Variations include: [Twitter Bootstrap Vanilla](https://github.com/anjlab/bootstrap-rails),
[Twitter Bootstrap with LESS](https://github.com/seyhunak/twitter-bootstrap-rails), or
[Twitter Boostrap SASS](https://github.com/thomas-mcdonald/bootstrap-sass).

For this tutorial, we will install Bootstrap with SASS. In your gemfile, include the following:

``` ruby Gemfile https://github.com/thomas-mcdonald/bootstrap-sass Link
gem 'sass-rails', '>= 3.2' # sass-rails needs to be higher than 3.2
gem 'bootstrap-sass', github: 'thomas-mcdonald/bootstrap-sass'
```
Now run ``` bundle install ``` in the command line. This will install version 3.0.1 of Twitter Bootstrap.

Open up your project, and within the folder **assets/stylesheets/**, rename **application.css** to **application.css.scss**. Now add ``` @import "bootstrap";``` anywhere in the file.<br>
In **assets/javascripts/**, make sure the file looks **EXACTLY** like this:
``` ruby application.js
//= require self
//= require bootstrap
//= require tree
``` This will preload all the javascript functionality that is included with Twitter Bootstrap.

## **Modifying Bootstrap**
Now that Twitter Bootstrap has been installed, we can customize the pages by calling in special classes provided by Bootstrap. What do I mean by *special classes*? This is where the **[documentation](http://getbootstrap.com/css/)** becomes important. Here is where I must stress the importance of following the documentation examples, so that you can easily learn and manifest the Twitter Bootstrap functionality.

## Examples
As of this post, I have been using Twitter Bootstrap for two months. In this short time, I have used the following functionality the most:<br>
**Jumbotron<br>**
**Grid System<br>**
**Buttons<br>**

Let's run through a quick implementation of these key pieces of Twitter Bootstrap.

### **[Jumbotron](http://getbootstrap.com/components/#jumbotron)**
This component adds a container component with specific styling to your page, which calls attention to anything written in it. It can be implemented easily by calling a "jumbotron" on a div. It looks like this:
``` ruby custom.css.scss
<div class="jumbotron">
  <h1>Hello, world!</h1>
  <p>...</p>
  <p><a class="btn btn-primary btn-lg" role="button">Learn more</a></p>
</div>
```
That jumbotron class is all that you need to specify to add this special functionality in your stylesheet! You can also see that there's another class in this code **"btn btn-primary btn-lrg"**. I'll introduce this now.

### **[Buttons](http://getbootstrap.com/css/#buttons)**
This component, as you have probably guessed, adds great looking buttons into your CSS. Taking the above example, ```<p><a class="btn btn-primary btn-lg" role="button">Learn more</a></p>```, this adds a button by calling "btn", followed by the color of the button "btn-primary", and lastly the size is specified with "btn-lg". There are **six** button colors: "btn-default", "btn-primary", "btn-success", "btn-info", "btn-warning", and "btn-danger". There are also four sizes: **Large**:"btn-lg", Default (doesn't require any specification), **Small**: "btn-sm", and **Extra Small**: "btn-xs".

### **[Grid System](http://getbootstrap.com/css/#grid)**
This is arguably the most important component of Twitter Bootstrap. This class specification uses a 12 grid system, with responsive scale, to add size and location to DIV classes. I suggest following the link and reading more about this class. The class is called by the following command:
``` ruby custom.css.scss
<div class="row">
  <div class="col-md-1">This is a medium size, 1 grid column</div>
</div>
```
The grid system comes in large, medium, small, and extra-small variations.

## Templates
Thanks to Twitter Bootstrap's popularity amongst web developers and designers, a number of free and paid-for templates exist for Twitter Bootstrap v2.3.2 and v3.0. These are a few of my favorite sites: <br>
**FREE:**<br>
[Bootswatch](http://bootswatch.com/)<br>
[Bootstrapstyler](http://bootstrapstyler.com/)<br>
**PAID:** <br>
[Wrap Bootstrap](ttps://wrapbootstrap.com/)<br>
In fact, it's easy to implement themes from **Bootswatch** by specifying in your ```application.css.scss``` file, ```@import "bootstrap/Flatly"```. This will import the **[Flatly](http://bootswatch.com/flatly/)** theme from Bootswatch in your project.<br>
I suggest diving deeper into all the benefits that Twitter Bootstrap provides. Try out **[Font-Awesome](http://fontawesome.io/)** and **[Glyphicons](http://getbootstrap.com/components/#glyphicons)**

## Links
If you like what you've seen so far, try your hand at implementing templates by following **[this](https://speakerdeck.com/derikulous/picasso-pirate)** tutorial I created.
