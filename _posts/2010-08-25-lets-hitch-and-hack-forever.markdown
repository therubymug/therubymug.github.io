---
title: "Let's Hitch and Hack Forever"
created_at: 2010-08-25 00:18:52.700027 -05:00
author: Rogelio J. Samour
layout: post
categories: blog
comments: true
---

I recently released `hitch` 0.6, a complete [rewrite](http://github.com/therubymug/hitch/tree/master) of the original `hitch`. We now have a test suite! :-) `hitch` is a Git author attribution helper for pair programmers. It will modify your git author name and email to reflect who you are pairing with.

# The itch we're scratching

[Les Hill](http://blog.leshill.org) originally wrote:

> Pair programming does present some unique problems, one that we encountered at [Hashrocket](http://www.hashrocket.com) was commit attribution: a commit message would be identified with one member of the pair only.  This is not exactly tragic, but from the perspective of a passionate developer, having commit credit (and accountability) is a critical and visible part of the ethos.

[Tim Pope](http://tpope.net) had the brilliant idea of using a group email address with tags (such as _group+you@example.com_) to identify the commits a pair made to git.  A little _bash script fu_ lead to a nifty command line utility that let us track pairs with our git commits.  With that script as a starting point, [Ro Samour](http://blog.therubymug.com) wrote a pure ruby implementation: `hitch`

This is what a typical `hitch` commit message looks like in your terminal:

{% highlight bash %}
commit e518dd0637e7d1d77d3bd79a645e5d0bc93eae2d
Author: Philip J. Fry and Turanga Leela <planex+fry+leela@example.com>
Commit: Bender Pairing Station <bender@example.com>

    Fix infite loop in empathy chip
{% endhighlight %}

This is a commit message from `hitch` on github:

![Github hitch commit message](/assets/github_commit.png)

# Getting started

You will need to install the `hitch` gem:

```% gem install hitch```

Or rvm users should copy and paste the following into your terminal:

{% highlight bash %}
for x in $(rvm list strings)
do
  rvm use $x@global && gem install hitch
done
{% endhighlight %}

Then you will need to add a shell function to your .bashrc or .zshrc:

{% highlight bash %}
hitch() {
  command hitch "$@"
  if [[ -s "$HOME/.hitch_export_authors" ]]; then
    source "$HOME/.hitch_export_authors"
  fi
}
alias unhitch='hitch -u'
# Uncomment to persist pair info between terminal instances
# hitch
{% endhighlight %}

Alternatively, you can also run the following:

```% hitch --setup >> ~/.bashrc```

*rvm users*: (that should be everyone ;-) You should add the shell commands from above _after_ you source the rvm scripts.

_Once that's done, source the ~/.bashrc or open a new terminal and you're good to go._

# The Group Email

You will need a group email account with a service such as [GMail](http://mail.google.com) or [GoogleApps GMail](http://www.google.com/apps/intl/en/business/index.html) that allows email tags[^1] (as an example, `twitter` is a tag in this email address _bender+twitter@example.com_ which is considered to be the same email address as _bender@example.com_):

At [Hashrocket](http://www.hashrocket.com,) we use a [Gravatar](http://gravatar.com) for our group email account so that the commits show the custom gravatar along with our commit messages. For every pair, we register a new gravatar and associate it with a tagged email address of the form:

```dev+github_user1+github_user2@example.com```

The github usernames should appear in the local part of the address in alphabetical order after the group address local part, each github username seperated by a `+`.

# Using `hitch`

To pair with another developer whose github username is `pair`:

```% hitch fry leela```

`hitch` will prompt you if it does not recognize github username of your pair and save it for later use.

To unpair and code solo:

```% hitch -u```

or

```% unhitch```

To see who you are currently paired with:

```% hitch```

[^1]: Read about email tags on [Wikipedia](http://en.wikipedia.org/wiki/E-mail_address#Sub-addressing)
