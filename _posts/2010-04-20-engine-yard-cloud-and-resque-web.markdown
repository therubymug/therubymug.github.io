---
title: "Engine Yard Cloud and Resque-Web"
created_at: 2010-04-20 17:13:16
author: Rogelio J. Samour
layout: post
categories: blog
comments: true
permalink: /blog/2010/04/20/engine-yard-cloud-and-resque-web.html
---

So at [Hashrocket](http://hashrocket.com/), we've been deploying a number of apps to Engine Yard Cloud.
Some of these apps have required the use of background jobs. A library that facilitates that is called
[Resque](http://github.com/defunkt/resque). Once set up[^1], it has a Sinatra web app you can use to monitor the queue's progress. To make a long story short, it isn't trivial to setup Resque Web on Engine Yard Cloud. This is what we did to make it work:

### In your environment.rb add the following line:

{% highlight ruby %}
Rails::Initializer.run do |config|
  #[...]
  config.middleware.use 'ResqueWeb'
end
{% endhighlight %}

### Then create a library under RAILS_ROOT/lib/resque_web.rb with the following:

{% highlight ruby %}
require 'sinatra/base'
class ResqueWeb < Sinatra::Base
  require 'resque/server'
  use Rack::ShowExceptions
  def call(env)
    if env["PATH_INFO"] =~ /^\/resque/
      env["PATH_INFO"].sub!(/^\/resque/, '')
      env['SCRIPT_NAME'] = '/resque'
      app = Resque::Server.new
      app.call(env)
    else
      super
    end
  end
end
{% endhighlight %}

That's it. Deploy your app to staging to test it and boom!

[^1]: This assumes you already have Resque installed in your EY Cloud app.
