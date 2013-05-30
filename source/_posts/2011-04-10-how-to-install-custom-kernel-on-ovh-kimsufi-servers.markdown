---
layout: post
title: How to install custom kernel on OVH / Kimsufi servers
categories:
- 网络
tags:
- kernel
- kimsufi
- linux
- ovh
published: true
comments: true
---
OVH servers are shipped with their specially tailored kernels. They are not bad but sometimes the applications (for example VirtualBox) do not agree with them. And they are also too old. Here is how to install custom kernels on these servers. Everything based on Ubuntu Server 10.10.

Most credits go to [this post](http://forums.ovh.co.uk/showpost.php?p=15454&amp;postcount=4). I updated some details and added the grub part.

So let's say you are going to compile and install kernel 2.6.38.2 (latest kernel by now) on a kimsufi server.

First of all, install Ubuntu server 10.10 in the managerv3, login with root.

<!-- more -->

Then:

{% codeblock lang:bash %}
apt-get install lzma
cd /usr/src
wget http://www.kernel.org/pub/linux/kernel/v2.6/linux-2.6.38.2.tar.bz2
tar xf linux-2.6.38.2.tar.bz2
cd linux-2.6.38.2
make mrproper
wget ftp://ftp.ovh.net/made-in-ovh/bzImage/2.6.34.6-3/2.6-config-xxxx-std-ipv6-32
cp 2.6-config-xxxx-std-ipv6-32 .config
make menuconfig
{% endcodeblock %}

At this step, you need to scroll to the end of the options and select "Load an Alternate Configuration File", load file .config, then go up and make sure "Enable loadable module support" is checked. Then go in "General setup", Clear whatever in the "Local version - append to kernel release". Now you can exit and save the config file.

Go on do the make. if you are on a multi-core server you can use the -j stuff. If not, drop that option.

{% codeblock lang:bash %}
make -j5 CONFIG_DEBUG_SECTION_MISMATCH=y
make modules
make modules_install
cp arch/i386/boot/bzImage /boot/bzImage-2.6.38.2
{% endcodeblock %}

Now here is different from the original post. OVH replaced lilo with grub. So the modifying lilo.conf method in the original post is no longer suitable. Instead, you need to alter the grub config file which is:

{% codeblock lang:bash %}
vim /boot/grub/grub.cfg
{% endcodeblock %}

Search for "OVHkernel" and change the bzImage file name in this block accordingly. Then save and exit.

Alright, you are good to go. Pray and type "reboot". Good luck.
