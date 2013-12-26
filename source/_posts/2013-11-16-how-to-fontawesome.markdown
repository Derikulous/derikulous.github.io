---
layout: post
title: "How to display FontAwesome in Firefox using Rails and CloudFront"
date: 2013-11-16 16:33
comments: true
categories: [FontAwesome, CloudFront, Ruby on Rails, troubleshooting, Firefox]
---
{% img http://25.media.tumblr.com/8f8d4d311eeca74a0496233c087dbbae/tumblr_mw6av9Opmb1rbnw5do2_500.gif "5 cm per second" %}

<h3> Winter is almost upon us; the perfect time for staying indoors and coding!</h3>
# Topics covered:
<ol>
  <li>Cloudfront configuration</li>
  <li>Cross-Origin Resource Sharing (CORS) gem</li>
</ol>
# What are we learning?
<p>
  In my <a href="https://derikulous.github.io/year/11/03/installing-twitter-bootstrap/"> earlier</a> post, I described how to install Twitter Bootstrap in your Rails project. Today, I was updating my <a href="http://www.derikstrattan.com/">portfolio</a> to use <a href="http://aws.amazon.com/cloudfront/"> CloudFront</a> as a content delivery service. After opening the page in Firefox, I noticed that the <a href="http://fontawesome.io/"> FontAwesome</a> icons were missing. This problem did NOT occur when the page was opened with Chrome or Safari. After more than 4 hours of troubleshooting, I had the page loading properly in Firefox. I sincerely hope that this post saves you countless hours and head scratching. Feel free to leave comments or suggestions. ^.^
</p>
# FontAwesome
<p>
  When you download FontAwesome, you will notice that there are 4 different files. What are these? FontAwesome comes in multiple formats so the maximum number of users (read: browsers) can see your fonts:
  <ol>
    <li>Safari & Opera browsers = .toff & .otf</li>
    <li>IE9+, Firefox, Chrome = .woff</li>
    <li>iOS and Android = .svg</li>
    <li>IE8 and older = .eot</li>
  </ol>
</p>
# HTTP Caching
<p>
  Before we dive head-first into setting up CloudFront, it's worth taking a slight detour to learn about HTTP caching and headers. According to <a href="https://devcenter.heroku.com/articles/increasing-application-performance-with-http-cache-headers"> Heroku</a>, "HTTP caching occurs when the browser stores local copies of web resources for faster retrieval the next time the resource is required. As your application serves resources it can attach cache headers to the response specifying the desired cache behavior." For our purposes, we need to understand <b>cache header</b>.
</p>
<p>
  Firefox and IE9+ reject all cross-site font requests unless some specific headers are set, i.e. ```Access-Control-Allow-Origin```. In other words, Firefox will not display a font if you use CloudFront to deliver content. For in-depth understanding, read more <a href="http://www.bryandragon.com/articles/rails-asset-pipeline-cdns-and-serving-cross-domain-fonts/">here</a> and <a href="http://stackoverflow.com/questions/11261805/rails-3-font-face-failing-in-production-with-firefox">here</a>. We'll need to figure out a way to allow Firefox access to the fonts.
</p>
# Configuring CloudFront
<p>
  {% img https://www.llamaproducts.com/assets/product_pics/decal_meat_n_taters.gif %} <br> Ahhh, we have finally arrived at the meat and 'taters of this article. Let's set up CloudFront (if you have questions, refer to this <a href="https://devcenter.heroku.com/articles/using-amazon-cloudfront-cdn-with-rails#testing"><i>post</i></a> on Heroku. But first, what <b>IS</b> CloudFront? Many developers use Amazon S3 (simple storage service) to store static assets (e.g. images, javascript, CSS). It is also sometimes used to <b>serve</b> those assets, but this is not recommended because the S3 was not designed to serve files when your website is under heavy traffic. CloudFront is referred to as a Content Delivery Network (CDN), and acts to serve those assets that you stored on S3.
</p>
## CloudFront setup
<p>
  <ol>
    <li>Make an <a href="http://aws.amazon.com">AWS account</a>.</li>
    <li>Login to your account, and go to <a href="https://console.aws.amazon.com/cloudfront/home">CloudFront control panel</a>. Click 'Create distribution', and when prompted for the delivery method, select 'Web'. For now, use defaults. NOTE: If you wish to use CNAME, you must enter your domain under 'Origin Domain Name'. </li><br> {% img https://s3.amazonaws.com/heroku.devcenter/heroku_assets/images/224-original.jpg %}
  </ol>
</p>

## Configure CloudFront and Rails
<p>
  1) In your Rails application, go to ``` config/environments/production.rb ```.<br>
  2) Look for the setting labelled ``` asset_host ```. This setting is responsible for prepending a domain onto all asset links created via the built in asset helpers.<br>
  3) In your CloudFront Management Console, select the distribution that you created. Click the link "Distribution Settings". Look for the subheader <b>Domain Name</b>, and copy that domain name. In your Rails app, paste in the domain: ``` config.action_controller.asset_host = "< YOUR DISTRIBUTION SUBDOMAIN>.cloudfront.net" ```.
</p>

## Purge content from CloudFront
We need to create an invalidation in the [CloudFront Panel](https://console.aws.amazon.com/cloudfront/home).To read more about why, go [here](http://www.red-team-design.com/firefox-doesnt-allow-cross-domain-fonts-by-default) <br>
Let's find our fontawesome assets. In your terminal, go to: ``` ~/project_name/public/assets ``` and type ``` ls -al fontawesome-webfont-*.* ```. You should see something like this:
``` ruby public/assets
fontawesome-webfont-4d228e1a1ddd570d6380dcd9b1a11d5c.ttf
fontawesome-webfont-6cfa562c51325bb1df40b64732dc8699.woff
fontawesome-webfont-9169b7b7c593388d41dd3c339df5b86b.svg
fontawesome-webfont-e742c0f297e307a74bdf4ca3c19073a2.eot
```

Back in the CloudFront Panel, choose the tab **Invalidations**. Click <button>Create Invalidation</button>.
<br>
{% img http://user-image.logdown.io/user/1/blog/317/post/52439/eWU50lLhRqWSbj89efo7_invalid1.png "blog.xdite" %}
<br>
Paste in the above assets like this:
``` ruby
assets/fontawesome-webfont-4d228e1a1ddd570d6380dcd9b1a11d5c.ttf
assets/fontawesome-webfont-6cfa562c51325bb1df40b64732dc8699.woff
assets/fontawesome-webfont-9169b7b7c593388d41dd3c339df5b86b.svg
assets/fontawesome-webfont-e742c0f297e307a74bdf4ca3c19073a2.eot
```
<br>
**IMPORTANT** Don't forget to add ``` assets/ ``` in front of the fontawesome.
<br>
Click <button>Invalidate</button>. Now wait for 5-10 minutes CDN cache to become invalidated.

# Install the CORS gem
In your Gemfile:
```
gem 'rack-cors', :require => 'rack/cors'
```
Run ``` bundle ```.
<br>
In your project folder, go to: ``` config/application.rb ``` and add the following:
<br>
``` ruby config/application.rb
module Portfolio
  class Application < Rails::Application
  # ...
    config.middleware.insert_before ActionDispatch::Static, Rack::Cors do
    allow do
      origins '*'
      resource '*',
      headers: :any,
      methods: [:get, :options]
    end
  end
  #...
    config.assets.paths << Rails.root.join("app", "assets")
    config.assets.precompile += %w( .svg .eot .woff .ttf )
    config.assets.header_rules = {
      :global => {'Cache-Control' => 'public, max-age=31536000'},
      :fonts  => {'Access-Control-Allow-Origin' => '*'}
    }
  end
end
```
<br>
The first part of this configuration puts the CORS gem at the top of your stack. Confirm this by running ``` rake middleware ``` in your terminal. Any header is now able to be retrieved via the ``` get ``` method. The second part precompiles the FontAwesome assets by specifically looking for their file extension. It also allows font control via the ``` header_rules ```.
## **Push to Heroku**
<p>
You are now ready to push everything up to Heroku. Don't forget to manually precompile assets, if necessary.
</p>
### Additional Resources
[Rails, S3, and asset sync](https://github.com/rumblelabs/asset_sync)<br>
[HTTP caching with Rails](https://devcenter.heroku.com/articles/http-caching-ruby-rails)<br>
[Rack::Cache](https://devcenter.heroku.com/articles/rack-cache-memcached-rails31)

<h3> What's next?</h3>
  In the next blog, I will explain how I moved this blog over from <a href="https://www.heroku.com/">Heroku</a> to <a href="http://pages.github.com/">GitHub pages</a>. If this post helped you, feel free to leave a comment. Thanks and good luck coding!
