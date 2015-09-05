---
title: The Install (Debian Edition)
author: Rogelio J. Samour
created_at: 2010-05-20 10:51:17
layout: post
categories: blog
comments: true
permalink: /blog/2010/05/20/the-install-debian.html
---
### Long story short

The following are steps we take at [Hashrocket](http://hashrocket.com) when setting up a near perfect Ruby development environment in GNU/Linux (Debian/Ubuntu). Feel free to copy and paste the commands into your terminal.

### A Word of Caution

While it is unlikely any instruction given below  might damage your system, it’s always a good idea to backup everything before doing any of it, just in case. I am not responsible for anything that may result from following the instructions below. Proceed at your own risk.

### Prerequisite Debian/Ubuntu Apps:

- [Chrome](http://www.google.com/chrome) (+ vimium, make default?)
- [Firefox](http://www.getfirefox.com) (+ firebug)

### Install vim

{% highlight bash %}
sudo apt-get install vim-gnome
{% endhighlight %}

### Install utilities

{% highlight bash %}
sudo apt-get install curl bison build-essential zlib1g-dev libssl-dev libreadline5-dev libxml2-dev
sudo apt-get install wget ack exuberant-ctags markdown gitk openssh-server
{% endhighlight %}

### get git

{% highlight bash %}
sudo apt-get install git-core
{% endhighlight %}

### Install [rvm](http://rvm.beginrescueend.com/rvm/install/)

{% highlight bash %}
bash < <(curl -s https://rvm.beginrescueend.com/install/rvm)

echo 'if [[ -s "$HOME/.rvm/scripts/rvm" ]] ; then source "$HOME/.rvm/scripts/rvm" ; fi' \
       >> ~/.bashrc.local

echo -e "export rvm_project_rvmrc_default=1
export rvm_install_on_use_flag=1
export rvm_project_rvmrc=1
export rvm_path='$HOME/.rvm'" > ~/.rvmrc

echo '[[ -r $rvm_path/scripts/completion ]] && source $rvm_path/scripts/completion' \
       >> ~/.bashrc.local # rvm autocompletion
{% endhighlight %}

### Setup your _global_ gems

A neat feature of rvm is defining the gems every _rvm-controlled_ ruby will install on its _global_ gemset. More [here](http://blog.therubymug.com/blog/2010/09/23/going-global-with-rvm-gemsets.html)

Simply add your desired gems to the file: ~/.rvm/gemsets/global.gems like so:

{% highlight bash %}
rake -v0.9.2
hitch
dirty
{% endhighlight %}

*Now* each time you `rvm install` a ruby version, you'll have hitch, dirty and rake version 0.9.2!

### Create your .gemrc by copying & pasting the following in your terminal:

{% highlight bash %}
echo -e '---
:benchmark: false
gem: --no-ri --no-rdoc
:update_sources: true
:bulk_threshold: 1000
:verbose: true
:sources:
- http://rubygems.org
:backtrace: false' > ~/.gemrc
{% endhighlight %}

### Install the rubies to your heart's content

{% highlight bash %}
rvm install 1.8.7
rvm install 1.9.2
rvm use 1.9.2 --default
{% endhighlight %}

### Install the Hashrocket dot files (don't worry, you can override your preferences)

{% highlight bash %}
mkdir ~/hashrocket
cd ~/hashrocket
git clone http://github.com/hashrocket/dotmatrix.git
{% endhighlight %}

### Symlink the good stuff!

{% highlight bash %}
ln -s ~/hashrocket/dotmatrix/.bashrc ~/
ln -s ~/hashrocket/dotmatrix/.hashrc ~/
ln -s ~/hashrocket/dotmatrix/.vim ~/
ln -s ~/hashrocket/dotmatrix/.vimrc ~/
sh ~/hashrocket/dotmatrix/bin/vimbundles.sh
{% endhighlight %}

### Mongo

Follow [these](http://www.mongodb.org/display/DOCS/Ubuntu+and+Debian+packages) instructions

### Postgresql (more in-depth install [here](http://blog.videntity.com/?p=523))

{% highlight bash %}
sudo apt-get install postgresql-common postgresql-server-dev-8.4 # Follow instructions
{% endhighlight %}

### MySQL

{% highlight bash %}
sudo apt-get install mysql-common mysql-server-5.1 # Follow instructions
{% endhighlight %}

### Generate a pubkey for that station

{% highlight bash %}
ssh-keygen -t rsa
# Add public key to your github account
{% endhighlight %}

### _Enjoy_

*update*: [2011-08-23] rvm install process, other misc.
