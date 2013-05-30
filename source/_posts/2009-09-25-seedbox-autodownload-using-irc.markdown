---
layout: post
title: 用irc脚本自动化Seedbox
categories:
- 互联网乱弹
tags:
- irc
- irssi
- kimsufi
- private tracker
- rtorrent
- seedbox
published: true
comments: true
---
前文再续是书接上一回，rtorrent跑起来了就该自动化了。什么？你觉得手动也行？那你一定很闲很闲吧......废话少说。用rss自动的方法就不提了，一是因为它太简单，二是因为它效果不好，跑TL之类的用rss还勉强，跑ScT就完蛋大吉了。BTW：ScT号俺已经搞到了，多谢那位朋友。

其实用irc脚本自动下载是很简单的事情，首先装个irssi:

{% codeblock lang:bash %}
sudo apt-get install irssi
{% endcodeblock %}

然后输入screen，进入另一个终端，输入irssi进去，连服务器，进入频道这些就不用说了吧。

下一步是找脚本，以TL的为例，在[这个帖子](http://forums.torrentleech.org/showthread.php?t=60151)里就有。所有tracker的脚本修改方法都大同小异。TL比较麻烦要cookie，刚才装的vnc就用上了吧？嘿嘿，到vnc里面打开firefox登录TL，然后把cookie给找出来填进脚本里面即可。ScT和What.CD的脚本没那么麻烦，一个填passkey一个填密码即可。最重要的修改点在torrent存放位置，一定要确认指向rtorrent的监控目录，否则就是鸡同鸭讲。

然后就是修改filter了，想刷全站自然也可以，不过疯狂了点，还是挑热门为上。每个站的脚本里头filter的写法都不大一样，照着样例改就是了，我的原则是宁缺勿滥，所以filter比较保守，大家根据自己实际情况定即可。

改好后保存，然后在irssi里头输入/load /path/to/the/script 即可

这些都做完了，就可以按 Ctrl+a, d 从screen里面退出来，然后logout了。以后想回到irssi的话，可以用screen -r切回去。
