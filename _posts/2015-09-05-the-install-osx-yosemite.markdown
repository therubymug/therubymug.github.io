---
title: The Install OS X (Yosemite)
author: Rogelio J. Samour
created_at: 2015-09-05
layout: post
comments: true
---

### Take-away

This post walks you through setting up a very sustainable [Ruby](http://ruby-lang.org) development environment on your Mac. You'll be able to write [Ruby on Rails](http://rubyonrails.org) apps along with anything else that uses Ruby. It is also the setup I use everyday.

### A Word of Caution

> As usual, while it is unlikely any instruction given below  might damage your system, it’s always a good idea to back up everything before doing any of it, just in case. I am not responsible for anything that may result from following the instructions below. Proceed at your own risk.

### Prerequisite OS X Apps:

- [Chrome](http://www.google.com/chrome) (+ vimium)

### Install Xcode or Command Line Tools

_homebrew_ needs for you to either install the [Command Line Tools for Xcode](https://developer.apple.com/downloads) or [Xcode](https://developer.apple.com/xcode) itself.

### Install homebrew

{% highlight bash %}
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
{% endhighlight %}

### Install utilities

{% highlight bash %}
brew install wget proctools ack ctags-exuberant markdown
{% endhighlight %}

### Install ImageMagick _(optional)_

{% highlight bash %}
brew install imagemagick
{% endhighlight %}

### Get git

{% highlight bash %}
brew install git
{% endhighlight %}

### dotfiles, dev folder

I personally like to use [this guy](https://github.com/therubymug/dotfiles)'s dotfiles.

Although, I strongly suggest that you fork the repo and modify it to suit your needs.

{% highlight bash %}
git clone https://github.com/therubymug/dotfiles.git ~/.dotfiles
cd ~/.dotfiles
script/bootstrap
{% endhighlight %}

### Install ruby-install and chruby

### Create your .gemrc by copying & pasting the following in your terminal:

{% highlight bash %}
echo -e '---
:benchmark: false
gem: --no-ri --no-rdoc
:update_sources: true
:bulk_threshold: 1000
:verbose: true
:sources:
- https://rubygems.org
:backtrace: false' > ~/.gemrc
{% endhighlight %}

### Install the rubies to your heart's content

{% highlight bash %}
ruby-install ruby 2.1.7
ruby-install ruby 2.2.3
{% endhighlight %}

### Postgresql

{% highlight bash %}
brew install postgresql # Follow instructions
{% endhighlight %}

### Generate a pubkey for this box

{% highlight bash %}
ssh-keygen -t rsa
# Add public key to your github account
{% endhighlight %}

# _Enjoy_
