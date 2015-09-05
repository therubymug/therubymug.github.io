---
title: The Install (Snow Leopard Edition)
created_at: 2010-05-20 15:51:17
author: Rogelio J. Samour
layout: post
categories: blog
comments: true
permalink: /blog/2010/05/20/the-install-osx.html
---

`update 2011-07-26`: The Install (Lion Edition) can be found [here](http://blog.therubymug.com/blog/2011/07/26/the-install-osx-lion.html)

### Long story short

The following are steps we take at [Hashrocket](http://hashrocket.com) when setting up a near perfect Ruby development environment in OS X Snow Leopard[^1]. Feel free to copy and paste the commands into your terminal.

### A Word of Caution

While it is unlikely any instruction given below  might damage your system, it’s always a good idea to backup everything before doing any of it, just in case. I am not responsible for anything that may result from following the instructions below. Proceed at your own risk.

### Step 0:

- [Uninstall MacPorts!](http://guide.macports.org/chunked/installing.macports.uninstalling.html)

### Prerequisite OS X Apps:

- [Chrome](http://www.google.com/chrome) (+ vimium, make default?)
- [Firefox](http://www.getfirefox.com) (+ firebug)
- [Fluid.app](http://fluidapp.com/)
- [Gitx](http://gitx.frim.nl/)
- [SizeUp](http://www.irradiatedsoftware.com/sizeup/)
- [Teleport](http://www.abyssoft.com/software/teleport/)
- [xCode](http://developer.apple.com/technologies/tools/xcode.html)

### Snow Leopard Samba Fix[^2]

{% highlight bash %}
echo "[default]" >  ~/Library/Preferences/nsmb.conf
echo "streams=no" >> ~/Library/Preferences/nsmb.conf
{% endhighlight %}

### Remove system gems

{% highlight bash %}
sudo rm -r /System/Library/Frameworks/Ruby.framework/Versions/1.8/usr/lib/ruby/gems/1.8
sudo gem update --system
sudo gem clean
{% endhighlight %}

### Install homebrew

{% highlight bash %}
sudo mkdir /usr/local
sudo chown -R `whoami` /usr/local
curl -L http://github.com/mxcl/homebrew/tarball/master | tar xz --strip 1 -C /usr/local
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

### Get git

{% highlight bash %}
brew install git
{% endhighlight %}

### dotfiles, dev folder

{% highlight bash %}
mkdir ~/hashrocket
cd ~/hashrocket
git clone http://github.com/hashrocket/dotmatrix.git

ln -s ~/hashrocket/dotmatrix/.bashrc ~/
ln -s ~/hashrocket/dotmatrix/.hashrc ~/
ln -s ~/hashrocket/dotmatrix/.vim ~/
ln -s ~/hashrocket/dotmatrix/.vimrc ~/
sh ~/hashrocket/dotmatrix/bin/vimbundles.sh
{% endhighlight %}

### Install [rvm](http://rvm.beginrescueend.com/rvm/install/)

{% highlight bash %}
mkdir -p ~/.rvm/src/ && \
  cd ~/.rvm/src && \
  rm -rf ./rvm/ && \
  git clone --depth 1 git://github.com/wayneeseguin/rvm.git && \
  cd rvm && \
  ./install
echo 'if [[ -s "$HOME/.rvm/scripts/rvm" ]] ; then source "$HOME/.rvm/scripts/rvm" ; fi' \
       >> ~/.bashrc.local
echo 'rvm_project_rvmrc_default=1' \
  > ~/.rvmrc # Use default gemset when leaving a .rvmrc project dir
echo '[[ -r $rvm_path/scripts/completion ]] && source $rvm_path/scripts/completion' \
       >> ~/.bashrc.local # rvm autocompletion
{% endhighlight %}

### Install openssl

{% highlight bash %}
rvm package install openssl
{% endhighlight %}

### Install the rubies to your heart's content

{% highlight bash %}
rvm install ree -C --enable-shared=yes
rvm install 1.8.7 -C --enable-shared=yes
rvm install 1.9.2 -C --enable-shared=yes
rvm use 1.8.7 --default
{% endhighlight %}

### Setup your .gemrc

{% highlight bash %}
echo -e '---
:benchmark: false
gem: --no-ri --no-rdoc
:update_sources: true
:bulk_threshold: 1000
:verbose: true
:sources:
- http://rubygems.org
- http://gems.github.com
:backtrace: false' > ~/.gemrc
{% endhighlight %}

### MySQL

{% highlight bash %}
brew install mysql # Follow instructions
{% endhighlight %}

### Postgresql

{% highlight bash %}
brew install postgresql # Follow instructions
{% endhighlight %}

If you get the following error:

{% highlight bash %}
@FATAL:  could not create shared memory segment: Cannot allocate memory@
{% endhighlight %}

Run this in your terminal window and try the @initdb@ command again.[^3]

{% highlight bash %}
sudo sysctl -w kern.sysv.shmall=65536
sudo sysctl -w kern.sysv.shmmax=16777216
{% endhighlight %}


### Install _global_ gems

{% highlight bash %}
for x in $(rvm list strings)
do
  # Uncomment the next 4 lines to install mysql, pg, & mongrel
  # rvm use $x@global && gem install mysql --\
  #  --with-mysql-dir=/usr/local \
  #  --with-mysql-config=/usr/local/bin/mysql_config
  # rvm use $x@global && gem install mongrel pg
  rvm use $x@global && gem install open_gem ruby-debug hitch
done
{% endhighlight %}

### RubyCocoa (for rspactor)

{% highlight bash %}
mkdir -p /usr/local/src
cd /usr/local/src
wget http://sourceforge.net/projects/rubycocoa/files/RubyCocoa/1.0.0/RubyCocoa-1.0.0.tar.gz/download
tar xzf RubyCocoa-1.0.0.tar.gz && rm RubyCocoa-1.0.0.tar.gz && cd RubyCocoa-1.0.0
ruby install.rb config --build-universal=yes
ruby install.rb setup
# have to sudo to install this because the installer wants to copy example stuff out of the /System dir
sudo ruby install.rb install
sudo chown -R `whoami` /usr/local # For good measure
{% endhighlight %}

### Growl

{% highlight bash %}
cd ~/Downloads
wget http://growl.cachefly.net/Growl-1.2.dmg
open Growl-1.2.dmg
cd /Volumes/Growl-1.2/Extras/growlnotify/
# Don't use the install shell script
# Use the following:
mkdir -p /usr/local/bin
echo "Creating /usr/local/bin"
mkdir -p /usr/local/man/man1
echo "Creating /usr/local/man/man1"
cp growlnotify /usr/local/bin/growlnotify
cp growlnotify.1 /usr/local/man/man1/growlnotify.1
{% endhighlight %}

### Generate a pubkey for that station

{% highlight bash %}
ssh-keygen -t rsa
# Add public key to your github account
{% endhighlight %}

[^1]: It may work for Leopard… I just haven't tried it yet.
[^2]: [More info](http://support.apple.com/kb/HT4017) (Thanks Alan Jones for the tip)
[^3]: [Found this here](http://willbryant.net/software/mac_os_x/postgres_initdb_fatal_shared_memory_error_on_leopard)
