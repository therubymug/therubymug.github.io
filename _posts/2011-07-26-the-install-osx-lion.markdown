---
title: The Install (Lion Edition)
author: Rogelio J. Samour
created_at: 2011-07-26 15:51:17.458279 -04:00
layout: post
categories: blog
comments: true
---

*update*: [2011-08-23] minor updates

*update*: [2011-11-05] I have automated this blog post into a shell script. Check it out [here](https://gist.github.com/1341873)

### TL;DR

- Copy & paste the following into a Terminal window:

{% highlight bash %}
test -f /tmp/the_install.sh && rm /tmp/the_install.sh
curl -s\
  https://raw.github.com/gist/1341873/60ccc411faadf1194941d7b65347ec09b10adac2/the_install.sh \
  -o /tmp/the_install.sh
chmod 0700 /tmp/the_install.sh
. /tmp/the_install.sh
{% endhighlight %}

### The saga continues

So this is the follow-up post to [The Install - Snow Leopard Edition](http://blog.therubymug.com/blog/2010/05/20/the-install-osx.html) which helped you, the [Ruby](http://ruby-lang.org) developer, set up a near-perfect development environment in Mac OS X. Only this time it's on Apple's latest version of Mac OS X, 10.7 (Lion). Again, feel free to copy and paste the commands into your terminal.

### Take-away

This post walks you through setting up a very sustainable [Ruby](http://ruby-lang.org) development environment on your Mac. You'll be able to write [Ruby on Rails](http://rubyonrails.org) apps along with anything else that uses Ruby. e.g. [Sinatra](http://www.sinatrarb.com,) [Padrino](http://www.padrinorb.com,) [Merb](http://www.merbivore.com/index.html,) et al. It is also the setup we use everyday at [Hashrocket](http://hashrocket.com.)

### A Word of Caution

As usual, while it is unlikely any instruction given below  might damage your system, it’s always a good idea to back up everything before doing any of it, just in case. I am not responsible for anything that may result from following the instructions below. Proceed at your own risk.

### Step 0:

- [Uninstall MacPorts!](http://guide.macports.org/chunked/installing.macports.uninstalling.html)

### Prerequisite OS X Apps:

- [Chrome](http://www.google.com/chrome) (+ vimium)
- [Firefox](http://www.getfirefox.com) (+ firebug, -note that Selenium is not yet compatible with Firefox 5-)
- [Fluid.app](http://fluidapp.com/)
- [Gitx](http://gitx.frim.nl/)
- [SizeUp](http://www.irradiatedsoftware.com/sizeup/)
- [Teleport](http://www.abyssoft.com/software/teleport/)

### Install Xcode

You _need_ to install Xcode for _homebrew_ to compile code from source. It's free in the Mac App Store! [Xcode](http://itunes.apple.com/us/app/xcode/id448457090?mt=12)

### OS X Named Streams issue[^1]

- Fix it system wide, with the following:

{% highlight bash %}
echo "[default]" | sudo tee /etc/nsmb.conf
echo "streams=no" | sudo tee -a /etc/nsmb.conf
{% endhighlight %}

### Install homebrew

{% highlight bash %}
sudo mkdir /usr/local
sudo chown -R `whoami` /usr/local
curl -L https://github.com/mxcl/homebrew/tarball/master | tar xz --strip 1 -C /usr/local
{% endhighlight %}

### Create .bash_profile

{% highlight bash %}
echo '. "$HOME/.bashrc"' > ~/.bash_profile
{% endhighlight %}

### Install MacVim

{% highlight bash %}
brew install macvim
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

{% highlight bash %}
mkdir ~/hashrocket
cd ~/hashrocket
git clone https://github.com/hashrocket/dotmatrix.git

ln -s ~/hashrocket/dotmatrix/.bashrc ~/
ln -s ~/hashrocket/dotmatrix/.hashrc ~/
ln -s ~/hashrocket/dotmatrix/.vim ~/
ln -s ~/hashrocket/dotmatrix/.vimrc ~/
sh ~/hashrocket/dotmatrix/bin/vimbundles.sh
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

### Setup your _global_ gems

 A neat feature of rvm is defining the gems every _rvm-controlled_ ruby will install on its _global_ gemset. More [here](http://blog.therubymug.com/blog/2010/09/23/going-global-with-rvm-gemsets.html)

Simply add your desired gems to the file: ~/.rvm/gemsets/global.gems like so:

{% highlight bash %}
rake -v0.9.2
hitch
dirty
{% endhighlight %}

*Now* each time you `rvm install` a ruby version, you'll have hitch, dirty and rake version 0.9.2!

### Install the rubies to your heart's content

{% highlight bash %}
rvm install 1.8.7
rvm install 1.9.2
rvm use 1.9.2 --default
{% endhighlight %}

### Mongo

{% highlight bash %}
brew install mongo # Follow instructions
{% endhighlight %}

### Postgresql

{% highlight bash %}
brew install postgresql # Follow instructions
{% endhighlight %}

### MySQL

{% highlight bash %}
brew install mysql # Follow instructions
{% endhighlight %}

### Generate a pubkey for this box

{% highlight bash %}
ssh-keygen -t rsa
# Add public key to your github account
{% endhighlight %}

# _Enjoy_

[^1]: [More info](http://support.apple.com/kb/HT4017)
