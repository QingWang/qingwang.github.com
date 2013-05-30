---
layout: post
title: 不推荐 -- PhotonVPS
categories:
- 网络服务
tags:
- linode
- photonvps
- prgmr
- vps
published: true
comments: true
---
半年前我购买了PhotonVPS的Bean 1 VPS，原因是价格便宜，据评测性能也不错。购买后主要用于建立SSH通道。

购买初期，VPS的表现尚可。然而随着时间的推移，VPS的反应越来越慢，在12月已经达到了输入一个字符要等一秒的地步。同时登录服务器时经常出现tty错误，每次都需要开ticket解决。最后一次居然把我的root密码搞丢了。

有鉴于此，我只能向大家不推荐PhotonVPS这家VPS供应商。

由此可验证，基于OpenVZ技术的VPS，由于可以轻易超售，其性能完全取决于主机商的良心，不靠谱。选择VPS的时候还是基于XEN技术的比较稳定。

现在我的SSH通道已经迁移到XEN VPS上。鉴于PhotonVPS给我留下的恶劣印象，自然不是他们家的。

我推荐的XEN VPS供应商为以下两家：

如果应用对性能及带宽要求较高，可以考虑Linode，他们家口碑极好，在多次VPS性能测试中都名列前茅，而且有美国4个机房加欧洲1个机房可选。

如果自己普通玩玩，Prgmr是个好去处，尽管性能上有所欠缺，提供的带宽也不如Linode，但价格优势明显，服务器也十分稳定。

嗯，最后加一句，如果是面向欧洲区的应用，那租VPS不如直接租Dedi，有很便宜的供应商。
