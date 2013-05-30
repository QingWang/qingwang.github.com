---
layout: post
title: "Octopress for Absolute Beginners"
date: 2011-11-19 09:55
comments: true
categories: tutorial 
---
### What is Octopress
[Octopress](http://octopress.org/) is a "blogging framework" according to its author. I'd rather call it a webpage publishing system. For its main function is to publish webpages.


### Why use Octopress
For blogging, most people use blogger, wordpress.com, livejournal, etc. Some build independent blogs with their own domain, own hosting and WordPress source code. Those are all very good solutions and one doesn't need to switch unless has really good reasons.

However if you, like myself, are already bored with all those popular platforms; or, again like myself, are planning to learn [Ruby](http://www.ruby-lang.org/en/), [Git](http://git-scm.com/) and are trying to work your way out of [GitHub](http://github.com/). Then Octopress may be for you.

By using Octopress you will get:

*  to learn how to build a Ruby programming environment.
*  some basic command of Git.
*  to write blog posts and pages completely with [CLI](http://en.wikipedia.org/wiki/Command-line_interface). That's right. You can write articles with VIM.
*  to use [markdown syntax](http://daringfireball.net/projects/markdown/basics) in blog posts. Or to learn it if you don't already know.
*  to host your blog on GitHub. Free.
*  very nice code highlighting.

...and, on top of all, it is fun.

<!--more-->

### I am convinced, how do I start?
It is not that difficult to setup Octopress even for absolute beginners. Although you will spend some time on try-and-error -- story of a coder's life.

I assume that you are working with a Mac. If you are not, go get one. 

First you will need to set up a Ruby environment and, of course, to install git. Octopress can only be run on Ruby 1.9.2 or later. OS X Lion's pre-installed Ruby is 1.8.7 and is not suitable for Octopress.

Installing Ruby 1.9.2 is very simple but I'd rather set up a complete Ruby on Rails development environment instead since I will soon need it anyway. As an absolute beginner myself, I followed [this walkthrough](http://wp.xdite.net/?p=2063). However there are a couple of mistakes in it. So I will just list all the commands again for future reference.

* Software update
* Install Xcode
* Install [Homebrew](http://github.com/mxcl/homebrew), Git and Imagemagick / MySQL / Sphinx
{% codeblock lang:bash %}
ruby -e "$(curl -fsS https://raw.github.com/gist/323731/install_homebrew.rb)" # The link in the origin post didn't work. I figured this is the right one.
brew install git
brew update
brew install imagemagick
brew install mysql
{% endcodeblock %}
Notice the version of MySQL. Then:
{% codeblock lang:bash %}
unset TMPDIR
mysql_install_db # This line is not right. Check the install log after the command "brew install mysql". The right command is in there. I forget what it is.
cp /usr/local/Cellar/mysql/5.1.54/com.mysql.mysqld.plist ~/Library/LaunchAgents # Replace the "5.1.54" with the MySQL version you installed just now.
launchctl load -w ~/Library/LaunchAgents/com.mysql.mysqld.plist
/usr/local/Cellar/mysql/5.1.54/bin/mysql_secure_installation # Again replace the version number.
{% endcodeblock %}
Now you need to initialise MySQL. Just answer Yes for all yes/no questions and give a password when asked.
{% codeblock lang:bash %}
brew install sphinx
{% endcodeblock %}

* Install RVM and Ruby 1.9.2
{% codeblock lang:bash %}
bash < <( curl https://raw.github.com/wayneeseguin/rvm/master/binscripts/rvm-installer ) # This one freaked me out a bit. It took me some time to find out the issue at http://bit.ly/tte5Iq
echo "[[ -s $HOME/.rvm/scripts/rvm ]] && source $HOME/.rvm/scripts/rvm" >> ~/.profile && . ~/.profile
source ~/.profile

rvm install 1.9.2 # Or 1.9.3 or later. If rvm doesn't work, try use the full path at ~/.rvm/bin/rvm
rvm use 1.9.2 --default
{% endcodeblock %}

* Install necessary gems
{% codeblock lang:bash %}
gem install rails
gem install mysql2
gem install mysql
gem install passenger
gem install nokogiri
gem install capistrano
gem install capistrano-ext
gem install delayed_job
gem install hoptoad_notifier
gem install facebooker2
gem install factory_girl
gem install sphinx
{% endcodeblock %}

Now the RoR environment is established. Next up: get Octopress work.

Actually, to this step you can just follow the [official Octopress document](http://octopress.org/docs/setup/) to finish the setup process provide that you are familiar with GitHub. If not, here is what you need to do:

* Register a GitHub account and log in. Please use **all lower-case letters** in your account name. Otherwise there may be problems when Octopress tries to connect to the repository the first time.

* [Create a new GitHub repository](https://github.com/repositories/new). Fill "Project Name" with your username. Again no capital letter. Description and Homepage URL can be left blank.

* Get your repository URL. This one is a little tricky. If you have not used GitHub before, you may go for the URL in the browser's address bar, which is wrong. The correct one can be find on the repository's page with the form "git@github.com:*repositoryname*/*username*.github.com.git".

* Now install Octopress. 
{% codeblock lang:bash %}
ruby --version # Make sure you have Ruby 1.9.2 or later in use.
git clone git://github.com/imathis/octopress.git octopress
cd octopress

gem install bundler
bundle install
rake install

rake setup_github_pages
{% endcodeblock %}

Now you will be asked for the GitHub Pages repository URL. Enter the one from last step. (Start with *git@*)

Open file *_config.yml* and fill the configs of *url*, *title*, *subtitle* and *author* as you see fit. Save and exit.

Then run:

{% codeblock lang:bash %}
rake generate
rake deploy
{% endcodeblock %}

Go grab a coffee and wait for the email from GitHub telling you the commit succeeded. The go on:

{% codeblock lang:bash %}
git add .
git commit -m 'some message'
git push origin source
{% endcodeblock %}

This should be it. Visit http://*username*.github.com/ to see whether it works. If it doesn't, try run generate and deploy again.

### Awesome! But how do I write posts?

Very simple:
{% codeblock lang:bash %}
rake new_post["your_post_title_here"]
{% endcodeblock %}

This command will generate a file with the name you specified under *source/_posts* . Just open it and write away. When finished, save and exit, then:

{% codeblock lang:bash %}
rake generate
rake deploy
{% endcodeblock %}

### What else should I know?
Congratulations. Now you know as much as I do. (That is to say, maybe 1% of Octopress.) Now go to the [official documentation of Octopress](http://octopress.org/docs/) and read the whole thing. At least learn how to put a custom domain on your blog. That's essential. Of course so is other stuff, such as creating a page and changing the favicon.

You should also learn [markdown syntax](http://daringfireball.net/projects/markdown/syntax). It's not hard and very useful.

Since I myself is also in beginner's stage I can not give more advices. I am sure you will know how to proceed after mastered the basis.

So that's it. Good luck and happy blogging on GitHub with Octopress.
