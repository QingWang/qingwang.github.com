---
layout: post
title: 还需要VPS么？
categories:
- 网络
tags:
- amazon
- ec2
- vps
- 云
published: true
comments: true
---
话说Shared hosting的时代俨然逐渐离去中，这年头爱折腾的都上VPS，贪着它有root，抗折腾，性能好等等。当然最重要的因素还是价格。Linode算是最流行的一各供应商，其VPS质量也确实过硬，是值得推崇的品牌。

不过呢，毕竟“云”才是大势所趋，互联网大鳄amazon的EC2已经推出很久了，当其时我便觉得很有取代VPS的可能性，EC2跟传统VPS相较基本全是优势，除了一点：价格。这一点足够要命了。

amazon近期强力推出aws micro instances也许能补上这方面的问题。与standard instances相比，micro instances没有reserved cpu time，内存为613MB (好奇怪的数字)，适合那些平时用cpu很少，有时候要burst一下的应用...我们常用的大部分应用都是这样的吧？似乎连burst都不多。

标价请参见[aws页面](http://aws.amazon.com/ec2/)。照这个算价格，跟Linode比，假设你开一个micro instances，那么对应的是Linode 512。如果你每月流量10GB，则：

Linode: 怎么都是20刀/月

amazon: 按加州机房价格算，reserved instance 54刀/年，加上1p/hour，就是0.01*24*365=87.6刀/年；再加上流量的10*12*0.15/GB=18刀/年，总共是54+87.6+18=159.6/年，也就是13.3刀/月，ooops，便宜了1/3。

简单计算可知，你的月流量要达到54.67GB的时候，两者价格相等。而Linode提供200GB的月流量，就是说如果你的流量在55GB-200GB/月之间的时候，Linode便宜，否则amazon EC2便宜。

EC2还有几个优势：第一你可以用Windows系统，稍贵些；第二你不用的时候可以关掉，就不算你钱，当然如果你放网站就不能关了；第三换ip方便，会心一笑；第四EC2现在有新加坡机房可选，我没测试过，按常理推论从中国访问起来应该比美国机房快。
