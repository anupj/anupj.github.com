---
layout: post
title: "Relaunching my Blog on Jekyll"
category: journal
comments: true
---

A new blog post is long due on my revamped blog, and I thought I'd start by writing a meta-blog-post about how to build a blog using [Jekyll][jekyll]. 

**Why Jekyll?**

I found out about Jekyll when I came across a blog post about Git on [Tom Preston-Werner's blog][toms blog]. When I checked out his code on Github, it led me to Jekyll,
and incidentally Tom is the creator of Jekyll. According to the Github page:

    Jekyll is a simple, blog aware, static site generator. It takes a template directory 
    (representing the raw form of a website), runs it through Textile or Markdown and 
    Liquid converters, and spits out a complete, static website suitable for serving 
    with Apache or your favorite web server. This is also the engine behind GitHub Pages, 
    which you can use to host your project’s page or blog right here from GitHub.

I liked Jekyll right off the bat because it meant I can write blog posts in my favourite text editor(Vim), and will be able to test it locally on
my machine before pushing the change out to the server. The second reason for choosing Jekyll was the ease of integration with Git and Github. It meant I can
use the DVCS I have come to love so much, and also be able to host my pages on Github. I was sold.

**Setup**

I used the following tools to build this blog:

  + MacVim editor - edit text at the speed of thought
  + homebrew - the missing package manager for OS X
  + rbenv - lightweight but powerful ruby version management
  + Git - one of the best DVCS

You might choose to use a different text editor or VCS, but this tutorial will focus on how to build a Jekyll site on Mac OSX which runs on [Github][github].

First step is to install the tools listed above if you don't have them on your machine already.
Open a terminal, and install homebrew using the following command:

    > /usr/bin/ruby -e "$(/usr/bin/curl -fsSL https://raw.github.com/mxcl/homebrew/master/Library/Contributions/install_homebrew.rb)" 

If you come across any issues, check out the [brew installation page][brew install] to see if you have the pre-requisites installed on your machine.
Then proceed to install rbenv using brew.

    > brew update
    > brew install rbenv
    > brew install ruby-build

Again, refer to the [rbenv github page][rbenv install], if you come across any issues during installation. rbenv will allow you to manage and install different versions
of ruby on your machine. Check out the Github project page for more details, and install the latest version of Ruby. You will also need to install [Rubygems][rubygems].

Let's install [git][git] using homebrew.

    > brew install git

Then create an account on Github.com, and follow [these steps][github setup] to link your git installation with your gitub account.
The final step is to install the Jekyll gem.

    > sudo gem update --system
    > sudo gem install jekyll

Jekyll comes bundled with Maruku markdown interpreter, but if you prefer to use RDiscount instead of Maruku, then install it as follows:

    > sudo gem install rdiscount

Now that you have all of the pre-requisites installed, you are ready to roll out a Jekyll site. 
I'd recommend forking one of the [existing jekyl sites][jblogs] on Github and tweaking it to give it your personality. 
It will also help you understand how the *framework* works. The Jekyll Github page has all of the information that you need to start building your blog.

Once, you have the directory structure and posts setup, run the following command in the blog root directory:

    > jekyll --server 4000

This will start the server, and _compile_ your posts and pages into corresponding html pages in the `_site` folder. You can preview your website in a browser
by typing `http://localhost:4000`.

The next step is to deploy your jekyll site to Github. To do this, you'd have to create a new repo on Github called (github-username).github.com. Then link this repo to 
your jekyll blog stored locally. You can find more information on how to do this [here][github pages]. This page will also tell you how to direct a domain name of your choice to your Github page.

That's it! You are good to go. If you need more information, or would like me to elaborate a particular step listed above, just drop a line in the comments. 

[jekyll]: https://github.com/mojombo/jekyll "Jekyll on Github"
[github]: https://github.com "Awesome DVCS"
[toms blog]: http://tom.preston-werner.com
[brew install]: https://github.com/mxcl/homebrew/wiki/installation
[rbenv install]: https://github.com/sstephenson/rbenv
[git]: http://git-scm.com
[github setup]: http://help.github.com/mac-set-up-git/
[jblogs]: https://github.com/mojombo/jekyll/wiki/sites "Existing Jekyll Sites"
[github pages]: http://help.github.com/pages/ "Github Pages"
