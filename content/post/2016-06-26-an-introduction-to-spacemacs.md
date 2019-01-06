---
title: An introduction to spacemacs
date: 2016-06-27
tags: ["emacs", "editor", "spacemacs"]
---

One of my joys as a developer is investing time tweaking and editing my own development tools. Not only is this therapeutic, but I get a buzz out of improving the efficacy of my work.
Recently I have been delving into the emacs community to see how the other side do it. After some experimentation, I wasn't really impressed and felt orders of magnitude slower than my current setup; then I found [spacemacs](http://spacemacs.org).

Spacemacs is amazing and it's actually really difficult to point to any single feature to explain why. An attempt though would be to reference a pretty solid quote by Aristotle:

> The whole is greater than the sum of it's parts

That is to say that spacemacs isn't good because of any single feature, but rather the coherence of all it's features together make it amazingly productive to use. To show this, I'm going to walkthrough a few different scenarios to demonstrate this.


#### Scenario 1 - Setting up

My first experience using spacemacs was surprisingly painless, this is partly due to the amazing documentation within emacs, but also the delightfully friendly and helpful community.

When you first spin up spacemacs, it nurtures you through an easy setup and downloads all the required dependencies through it's very own package manager. You will be asked a few easy question, namely the editing style that you prefer. There are options for both modal editing (evil mode: vim) and regular editing modes (holy mode: emacs, atom, sublime text) and both configurations work brilliantly out of the box. 

Before we go any further, it's worth mentioning how easy it is to get up and running with spacemacs:

```
$ brew tap d12frosted/emacs-plus
$ brew install emacs-plus --with-cocoa --with-gnutls --with-librsvg --with-imagemagick --with-spacemacs-icon
$ brew linkapps
$ git clone https://github.com/syl20bnr/spacemacs ~/.emacs.d
```

You can then open up the GUI emacs application, or run `emacs` from cmd line.

#### Scenario 2 - Making it my own

The beautiful part about using extensible software is making it your own. Spacemacs while being a community driven configuration, really just provides super strong defaults for getting started. Out of the box spacemacs works like vim; quite well I might add. But there were still some changes I've made to my configuration that suited me. Here's the short list:

Enabled some cool layers

```lisp
dotspacemacs-configuration-layers
'(auto-completion
  better-defaults
  emacs-lisp
  javascript
  react
  git
  markdown
  org
  (shell :variables
          shell-default-height 30
          shell-default-position 'bottom)
  ;; spell-checking
  syntax-checking
  version-control)
```

Added solarized dark colorscheme

```lisp
dotspacemacs-themes '(solarized-light
                      solarized-dark
                      zenburn
                      monokai)
```

Added `jk` as my `evil-escape-sequence`. This does what `ESC` does in vim-mode

```lisp
(setq-default evil-escape-key-sequence "jk")
```

#### Scenario 3 - Getting help

Help is never far away using emacs. In fact, with the power of `which-key`, every command stroke will provide a guide for 'which' key does what. For example, press `SPC` while in normal mode and you'll get something that looks like this:
<center><amp-img width="500" height="300" layout="responsive" src="/assets/images/spacemacs-which.png"></amp-img></center>

Spacemacs bindings are extremely intuitive, for example `SPC b` gives you buffer commands. `SPC f` gives you file commands etc. Let's say that you want to know what a keybind does, well you can press `SPC h d k` which stands for `help describe key`. After typing this, enter a command sequence to receive a detail overview of what it does. I leave further exploring up to the reader.

If you want to read the spacemacs documentation, you can also easily navigate to it via `SPC h SPC`, which stands for `help spacemacs`. From here you can see the self documented spacemacs documentation and overview. Neat!
<center><amp-img width="500" height="300" layout="responsive" src="/assets/images/spacemacs-help.png"></amp-img></center>

Even if you use vim style, you may find the default emacs `M-x` (meta being alt/option on OSX) to be useful. Especially since `SPC` only works when in normal mode. 

Last but not least, if you still can't find answers to problems, the spacemacs community is extremely helpful. I personally recommend their <a href="https://gitter.im/syl20bnr/spacemacs">Gitter</a> or open up a ticket on Github. Oh did I mention, you can do that from within spacemacs as well? See if you can find the key combination; if you get stuck try `SPC ?` and fuzzy find `issue`.

#### Scenario 4 - Writing Code

Spacemacs is a powerhouse for writing code. It has all the bells and whistles out of the box. `SPC p p` let's you navigate between projects. `SPC p f` let's you search for files within a project. `SPC p t` opens up a file tree. Autocompletion support is beautiful and searching and refactoring is a breeze using `SPC s`. Keep in mind, spacemacs gives you all the power of vim, within a strong ecosystem of tools.

#### Scenario 5 - Git management

Spacemacs comes with magit. Magit is well, the best git integration tool I've ever used. Lately, our team has been preferring rebasing over merging and magit makes this process extremely easy. Inside of a git project, just type `SPC g s` to bring up `git status`. Inside of this buffer you can press `f` for fetch, or `F` for pull. `c` for commit, `p` for push, `r` for rebase, `l` for log. Each of these commands brings up different interfaces and the best thing is, if you get stuck just press `?`.
<center><amp-img width="500" height="300" layout="responsive" src="/assets/images/spacemacs-git.png"></amp-img></center>

This is really just the tip of the iceberg and you can do some really amazing things with magit. Do some research over at https://github.com/magit/magit, but if you don't believe me, at least try out the 3-way merging or the discarding of particular hunks in a diff. They blow the mind.

#### Scenario 6 - Pair programming

One of the really difficult parts of using vim is being able to pair program with people who are not used the keybindings. 

<center><blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">I&#39;ve been using Vim for about 2 years now, mostly because I can&#39;t figure out how to exit it.</p>&mdash; I Am Devloper (@iamdevloper) <a href="https://twitter.com/iamdevloper/status/435555976687923200">February 17, 2014</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script></center>

Spacemacs has a simple solution to this, *turn it off* with `SPC t E h` or `toggle editing-mode holy`. Once in this mode, any non-vimmers should be clear of confusing modal editing, with the same layers and tools that you've been using. Alternative, you can use `Ctrl-Z` which toggles evil mode.

### Wrapping Up

Spacemacs is a well thought out, intuitive and performant configuration of emacs, backed by the editing efficacy of vim. Though it seems like spacemacs adheres to *convention over configuration*, it really has all the freedom and extensibility of vanilla emacs, backed by an intelligent set of community driven defaults. Enjoy!
