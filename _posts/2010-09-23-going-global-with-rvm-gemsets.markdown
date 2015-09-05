--- 
title: Going Global with RVM Gemsets
author: Rogelio J. Samour
created_at: 2010-09-23 20:48:17
layout: post
categories: blog
comments: true
permalink: /blog/2010/09/23/going-global-with-rvm-gemsets.html
--- 

I wanted to share briefly on installing [global or default gems](https://rvm.io/gemsets/initial) in your rvm-controlled rubies. So here it is, say you wanted to always have a certain list of gems installed in all versions of ruby that your rvm controls. Right now you'd have to do something like this to install one or more gems in all your rubies:

{% highlight bash %}
for x in $(rvm list strings)
do
  rvm use $x@global && gem install hitch bundler
done
{% endhighlight %}

And the above works great for your existing rubies. However, going forward the best thing to do is to make your ~/.rvm/gemsets/global.gems look like so:

{% highlight bash %}
bundler
hitch
mysql
rake
{% endhighlight %}

Perfecto! Now the next time you install a new ruby version, bundler, hitch, mysql and rake will be available across all [gemsets](https://rvm.io/gemsets/basics)

{% highlight bash %}
$ rvm install 1.9.2

[...] blah blah Installing Ruby blah blah [...]

$ rvm use 1.9.2
Using ~/.rvm/gems/ruby-1.9.2-p0
$ gem list

*** LOCAL GEMS ***

bundler (1.0.0)
highline (1.6.1)
hitch (0.6.0)
mysql (2.8.1)
rake (0.8.7)

$
{% endhighlight %}

*Now* you can do the following to retrofit your currently installed rubies:

{% highlight bash %}
for x in $(rvm list strings)
do
  rvm use $x@global && gem install $(cat ~/.rvm/gemsets/global.gems)
done
{% endhighlight %}

*For more info on how to get your environment setup to use rvm in all its glory, go [here](http://blog.therubymug.com/blog/2010/05/20/the-install-osx.html*).
