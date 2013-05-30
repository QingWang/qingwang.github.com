---
layout: post
title: "Rails on Ubuntu VPS"
date: 2012-03-19 10:05
comments: true
categories: Tutorial
---
So now I have a KVM VPS running Ubuntu. How do I setup a rails development environment? The Linux/OSX users are no as lucky as the Windows users at this -- There is no simple stupid all-in-one script/binary for us. Most of us need to go through a try-and-error process to accomplish the task. In this post I am going to do it again and try to record it so that other people (or the future me) can playback.

###Prepare

That's the routine commands:

{% codeblock update system lang:bash %}
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install git curl build-essential vim libcurl4-openssl-dev bison openssl libreadline6 libreadline6-dev zlib1g zlib1g-dev libssl-dev libyaml-dev libsqlite3-0 libsqlite3-dev sqlite3 libxml2-dev libxslt-dev autoconf

{% endcodeblock %}

###ZSH

This step is optional. Bash is good enough. However I like the fancy zsh for its flexible custom prompt.

{% codeblock install oh-my-zsh lang:bash %}
sudo apt-get install zsh
git clone git://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh
cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc
chsh -s /bin/zsh
{% endcodeblock %}

Personally, I will change the zsh theme to "gnzh". You can leave it as is or use any theme you prefer. To do this, open ~/.zshrc and alter "ZSH_THEME".

<!-- more -->

###Install Ruby Version Manager (RVM)

{% codeblock install rvm lang:bash %}
bash -s stable < <(curl -s https://raw.github.com/wayneeseguin/rvm/master/binscripts/rvm-installer)
source ~/.rvm/scripts/rvm
{% endcodeblock %}

Then open ~/.zshrc (or ~/.bashrc if you use bash) and add the following line **before** the line of "ZSH_THEME":

{% codeblock add rvm to path lang:bash %}
PATH=$PATH:$HOME/.rvm/bin
{% endcodeblock %}

Then add this line to the end of the file:

{% codeblock startup env lang:bash %}
[[ -s "$HOME/.rvm/scripts/rvm" ]] && . "$HOME/.rvm/scripts/rvm"
{% endcodeblock %}

###Install Ruby

{% codeblock install ruby 1.9.3 lang:bash %}
rvm install 1.9.3
rvm --default use 1.9.3
{% endcodeblock %}

###Install Rails
{% codeblock install rails lang:bash %}
sudo apt-get install install libbuilder-ruby
gem install rails
{% endcodeblock %}

###Install Passenger with Nginx
{% codeblock install passenger and nginx lang:bash %}
gem install passenger
sudo -s
passenger-install-nginx-module
{% endcodeblock %}
Enter to continue; choose 1 (Yes: download, compile and install Nginx for me. (recommended))
Here the installation script thrown an error at me. It could not find "pcre-8.12". Turns out this version is too old and has already been removed from the source server. I have to install it manually.
Goto ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/ and download the latest version from there. At this moment it is pcre-8.30
{% codeblock install pcre lang:bash %}
wget ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre-8.30.tar.gz
tar zxvf pcre-8.30.tar.gz
cd pcre-8.30
./configure
make
sudo make install
{% endcodeblock %}
Now try "sudo -s ; passenger-install-nginx-module" again. It should work this time.

Specify prefix directory of nginx. The default /opt/nginx is fine. Some prefer /usr/local/nginx . Not much difference.

Note: if passenger-install-nginx-module can not be found, use the full path.
 
(/home/*username*/.rvm/gems/ruby-*(version)*/gems/passenger-*(version)*/bin/passenger-install-nginx-module)

###Nginx startup script
{% codeblock install and test nginx startup script lang:bash %}
sudo mkdir /opt/nginx/init.d
sudo wget --no-check-certificate http://github.com/ascarter/nginx-ubuntu-rvm/raw/master/nginx -O /opt/nginx/init.d/nginx
sudo chmod +x /opt/nginx/init.d/nginx
sudo ln -s /opt/nginx/init.d/nginx /etc/init.d/nginx
sudo ln -s /usr/local/lib/libpcre.so.1 /usr/lib/libpcre.so.1
sudo /etc/init.d/nginx start
sudo /etc/init.d/nginx status
sudo  /etc/init.d/nginx stop
sudo /usr/sbin/update-rc.d -f nginx defaults
{% endcodeblock %}

Note: if you installed nginx in a place other than /opt/nginx, open the script file and change the path of *DAEMON*, *PIDSPATH*, *NGINX_CONF_FILE*.

###Install rmagick
{% codeblock install imagemagick and rmagick lang:bash %}
sudo apt-get remove imagemagick
sudo apt-get install libperl-dev gcc libjpeg62-dev libbz2-dev libtiff4-dev libwmf-dev libz-dev libpng12-dev libx11-dev libxt-dev libxext-dev libxml2-dev libfreetype6-dev liblcms1-dev libexif-dev perl libjasper-dev libltdl3-dev graphviz gs-gpl pkg-config
wget ftp://ftp.imagemagick.org/pub/ImageMagick/ImageMagick.tar.gz
tar zxvf ImageMagick.tar.gz
cd ImageMagick-*(version)*
./configure
make
sudo make install
gem install rmagick
{% endcodeblock %}

###Test
{% codeblock create a new rails app for env test lang:bash %}
rails new testapp
cd testapp
git init
git add .
git commit -a -m "Initial import"
bundle install
git add .gitignore
git commit -a -m "Add .gitignore"
{% endcodeblock %}

Edit /opt/nginx/conf/nginx.conf (or your specified path to nginx.conf)
{% codeblock virtual host config that needs to be added to nginx.conf lang:bash %}
   server {
      listen 80;
      server_name www.yourhost.com;
      root /somewhere/public;   # <--- be sure to point to 'public'!
      passenger_enabled on;
   }
{% endcodeblock %}

Then restart nginx, try visit the server from browser.

###Databases
MySQL:
{% codeblock install mysql lang:bash %}
sudo apt-get install mysql-client mysql-server
gem install mysql
{% endcodeblock %}

PostgreSQL:
{% codeblock install postgresql lang:bash %}
sudo add-apt-repository ppa:pitti/postgresql
sudo apt-get update
sudo apt-get install postgresql libpq-dev
{% endcodeblock %}
