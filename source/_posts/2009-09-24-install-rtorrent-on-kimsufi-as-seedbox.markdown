---
layout: post
title: 在kimsufi服务器上安装rtorrent做seedbox的步骤
categories:
- 互联网乱弹
tags:
- kimsufi
- private tracker
- rtorrent
- seedbox
published: false
comments: true
---
#### 为什么用kimsufi？ ####

A：因为它便宜或者说性价比高。这是显而易见的一点。
B：因为大家都用它。看到这个理由，了解PT工作原理的人应该会心一笑吧。

#### 为什么用rtorrent？ ####

实际上我是先尝试了wine+utorrent的，能成功跑起来，wine+ut的好处是资源占用小，功能齐全，设置方便。但也有其缺点，就是目录监视自动下载有时候会不动，而且种子启动时连peers慢。权衡之下我还是觉得rtorrent稍微靠谱些。除了原生主义外，葡头告诉我Sct这个盒子站rt的效率高，也是个重大因素，虽然我还没Sct的号。(BTW: 谁如果有邀请的话砸一个吧。)

本文前半部分参考[这篇教程](http://filesharingtalk.com/vb3/f-guides-and-tutorials-65/t-naqs-complete-setup-guide-linux-seedboxes-fedora-corecentosdebianubuntu-281331)，修正了不少。

第一步：租个kimsufi服务器，方法不在本文讨论范围内，略。

第二步：环境配置

- **租服务器的时候，选Ubuntu 8.10 english**，这是我一次一次重灌系统得出的经验，debian 5, ubuntu 9.04以及centos 5.3都会出问题，只有ubuntu 8.10能顺利完成所有步骤。当然这是我功力有限导致的。但既然有一个系统能简单完成，我们就不必没有困难创造困难了。
- 一开始root登陆，马上 passwd 换掉密码，rm .ssh/authorized_keys2 删掉这个key，SSHD需要进一步安全配置，不过现在先这样，最后再说。/etc/init.d/ssh restart 重启SSHD。SELinux和iptables缺省都没有安装，很好，我们现在不需要。
- 更新系统软件包，运行 apt-get update 和 apt-get upgrade 。
- 把需要的东西装好：

{% codeblock lang:bash %}
apt-get install vnc4server xterm fluxbox vsftpd firefox curl
apt-get install xfonts-base xfonts-75dpi xfonts-100dpi
update-menus
{% endcodeblock %}

wine不用装，因为我们不用utorrent，如果你想用utorrent就得装wine，不提。

- 搞搞vnc，为什么我们不用wine还要搞vnc？其实是可以不用的，但有些PT站的IRC下载脚本要拿cookie，用 vnc里头运行一下浏览器可以方便地得到cookie。当然我相信用curl之类的肯定更快更好，不过我这不是不懂嘛，呵呵。而且有个远程桌面环境也不是什么坏事。

{% codeblock lang:bash %}
cd
mkdir .vnc
vi .vnc/xstartup
{% endcodeblock %}

里头加一行 fluxbox 然后:wq

{% codeblock lang:bash %}
chmod +x .vnc/xstartup
vncserver :1 //这行就启动vnc了，设定一个连接密码。要关闭，用 vncserver -kill :1
{% endcodeblock %}

如果追求安全可以用vncserver -localhost :1，然后用putty搭桥过去，不过没必要，用完关掉就是了。

- 先试试vnc好不好用再说，到[这里](http://www.realvnc.com/cgi-bin/download.cgi)下载个vncviewer然后填上box的ip连过去试试看，连通后右键菜单里头找firefox看能不能运行。都顺利完成之后，就先放一边吧，呆会再用它。
- 该装rtorrent了，如果你要自己装的话可能会抓狂，不推荐，既然有现成的安装脚本就应该好好利用。我是在what.cd的论坛里面找到这个好东西的，只需这样：

{% codeblock lang:bash %}
cd
wget http://76.76.238.18/wtorrent-installer.tar
tar xf wtorrent-installer.tar
{% endcodeblock %}

按说下一步就该运行脚本了...错，先把配置文件改好再说。

{% codeblock lang:bash %}
vi wtorrent-installer/.rtorrent.rc
{% endcodeblock %}

怎么改呢？我推荐这样：

on_finished = move_complete,......这一行注释掉。你要移动要重新做种，不好。

min_peers这一些就酌情改改即可。

download_rate设成7000，全速下载的时候就不能全速上传。如果你租的是kimsufi或者ovh的高配服务器，可能不用改。

放文件的目录和自动下载的监视目录自己看着办。

port_range我喜欢改后一点，不过随便。

check_hash=no 一定要no，否则亏很大，下载完了hash一边再做种，渣都没得留给你了。

加密能加就加吧。

dht=off;peer_exchange=no，这两个没啥可说的，一定要这样设。

别的就不用管了。:wq吧

- 总算可以运行安装脚本了 wtorrent-installer/install.sh 这个脚本给你直接都装好开始运行，所以就不用费别的心了。访问 http://你的ip/ 去设置一下wtorrent(就是rtorrent的web前端界面)就能直接用了。注意会给你新建一个叫rt的用户。

如若你改了配置文件要重启rtorrent，用命令 /etc/init.d/rtorrent restart

第三步：自动化一下，否则没啥意义。

嗯...今天不想写了，下一篇再说吧。

P.S. 这种seedbox刷HDC一天200GB很轻松，CHD这种有2x的就更不用说了，TL之类的更是三天1T没问题。只有What.CD...对付不了。
