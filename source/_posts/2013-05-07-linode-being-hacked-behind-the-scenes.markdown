---
layout: post
title: "Linode 信用卡信息泄露的幕后故事"
date: 2013-05-07 13:40
comments: true
categories: 
- 互联网乱弹
tags:
- security
- story
---
上个月 [Linode 被骇客入侵](https://blog.linode.com/2013/04/16/security-incident-update/)导致所有信用卡信息泄露，我也未能幸免地中招去换了卡。今天在 HN 看到[侵入者所发布的幕后故事](http://straylig.ht/zines/HTP5/0x02_Linode.txt)，觉得挺有意思，就转过来跟大家分享一下。

我对骇客界的术语完全不懂，以下转译基于 [HN 上的一条评论](http://straylig.ht/zines/HTP5/0x02_Linode.txt)，掺杂一些我自己的理解。

HTP ("Hack The Planet") 是一个骇客组织，某不知名的团体装成另一个叫 ("ac1db1tch3z") 的组织对 HTP 进行了一些调查活动，主要是想寻找 HTP 使用的 [Botnet (僵尸网络)](http://en.wikipedia.org/wiki/Botnet)。而这行为被 HTP 察觉到了，于是打算进行报复。

<!-- more -->

HTP 发现这个不知名的团体在 [SwiftIRC](http://www.swiftirc.net/) 上交流 (不懂什么是 IRC 的就想像成 QQ 群吧)。如果 HTP 可以控制 SwiftIRC ，就可以监听控制这个团体的交流；而 SwiftIRC 使用的服务器恰好是 Linode 提供的，所以 HTP 决定黑进 Linode ，取得底层服务器的根权限后，想干什么自然是轻而易举了。

然而不知基于什么原因，也许是没有找到 Linode 的破绽， HTP 一开始并未直接对 Linode 下手，而是从 Linode 的域名注册商 [Name.com](http://www.name.com/) 开刀，黑进了 Name.com 的后台取得了 Linode 域名的控制权限。所以说 HTP 一路找上游，路径看起来像这样：

Name.com -> Linode -> SwiftIRC -> 不知名团体

(顺带一提， HTP 原文中说到很多域名注册商，包括 Name.com , 新网 等等都在他们的控制之中。)

控制了域名以后， HTP 的原计划是把 Linode 用户导向他们自己的网站，在跳转到 Linode 前截下用户密码。这计划看上去是个很大的动作，想不被人发现似乎可能性很小。

不过在 HTP 实施这个计划之前，他们的一位成员发现了 Linode 使用的一个名叫 ColdFusion 软件的一个漏洞，利用这个漏洞， HTP 直接侵入了 Linode 的服务器，域名劫持计划最终并没有实施，因为已经不需要了。 HTP 顺利取得了 SwiftIRC (以及所有放在 Linode 上的网站，包括知名安全网站 nmap.org) 的控制权。

然而！ FBI 卧底遍天下之名岂是空谈？就在 HTP 小组中，FBI 早就安插了眼线。此卧底发现 HTP 居然控制了 nmap ，觉得事态严重，于是暗中提醒 Linode ，「大哥醒醒你被黑掉啦」。

这样一来 HTP 就面临一个问题，原本他们想神不知鬼不觉地控制这个不知名团体，但是现在 Linode 已经了解事态，肯定要公开通告用户，这样一来那个团体必然有所警觉，一旦他们挪窝，岂不是白忙活了？

HTP 于是直接跟 Linode 谈判，他们要求 Linode 保持沉默到五月一号，交换条件是 HTP 会把从 Linode 得到的信用卡信息全部删除，否则就把信用卡信息公开。

Linode 方面也许想过妥协，但是这根本由不得 Linode 做主，FBI 强制 Linode 必须立刻通告用户。 HTP 对此也没有办法，于是改为要求 Linode 在通告中说明这事情是 HTP 干的 (我咋听起来这么像栽赃…) ，然后 (据他们自己说) 删掉了信用卡信息。

至于 HTP 后来有没有监控到那个不知名团体，就不清楚了。HTP 事后对小组成员进行了调查整肃，据说他们黑进了那个 FBI 卧底的电脑，打开了他的摄像头，看到他身后的 FBI 探员，于是把他踢出了 HTP .

好吧，这是一个来自 HTP 单方面的故事，能相信多少我也不知道，就当是一点娱乐吧。