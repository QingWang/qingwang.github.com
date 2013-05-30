---
layout: post
title: DIY 家用 NAS
categories:
- 网络
tags:
- diy
- freenas
- nas
- storage
published: true
comments: true
---
### 1. 为什么需要 NAS ###

随着科技进步，普通家庭中的智能设备越来越多，包括传统的台式机，笔记本电脑以及新潮的平板电脑，智能手机甚至媒体播放器(把 iPod touch 归为媒体播放器有点怪异，但人家确实是 iPod 家族成员...)。而便捷地在这些设备之间共享文件的需求也日益增加，于是原本属于专业设备的 [NAS (Network-Attached Storage)](http://en.wikipedia.org/wiki/Network-attached_storage) 逐渐进入普通家庭中。

NAS 相当于大大扩充了家庭中每一台智能设备的存储容量，同时也让它们都能随时独立访问任何文件。简而言之，就是“扩容”和“共享”两大功能。如何利用这功能，则看用户需求了。有些人把 NAS 当作多媒体库使用，存放大量的影视音乐文件；有些人则将其作为备份仓库，存放数码照片原片以及视频原始脚本等。总而言之，可以将其看做一块无线连接在所有智能设备上的大硬盘，随便如何使用。

<!-- more -->

### 2. 家用 NAS 方案 ###
在专业的 NAS 使用场合，为了数据安全和连接可靠，一般使用昂贵的专业解决方案。而作为家用计划，不可能负担动辄上万美刀的专业设备。摆在家庭用户面前有两条路：直接购买家用 NAS 成品设备，或者 DIY 一台 NAS 。

这两个选择各有利弊，没有什么绝对的优劣之分。不过作为一个手头拮据而且对外观毫无要求的男性资深电脑爱好者，本人的选择应当是一目了然的了。唯一的问题是我生活在折合人民币两块六一度电的神奇国家， DIY 的 NAS 耗电量令人头疼。这点上不可能达到成品设备水平，只能设法尽量接近了。

综合许多意见之后，最终配件清单如下：

- £104 [Asus E35M1-M Pro](http://www.amazon.co.uk/gp/product/B004KX216E/)
- £42 [Corsair CMX4GX3M1A1333C9 XMS3 Desktop Memory 4GB](http://www.amazon.co.uk/gp/product/B003ZDJ42O/) * 2	
- £32 [Cooler Master RC-370-KKN1 Elite 370 Midi Tower PC Case](http://www.amazon.co.uk/gp/product/B003OEMH28/)
- £47 [Be Quiet! Pure Power 350W PSU](http://www.amazon.co.uk/gp/product/B001KN8CVA/)
- £219 [Hitachi 2TB 3.5" Deskstar Coolspin HDS5C3020ALA632](http://www.ebuyer.com/254429-hitachi-2tb-3-5-deskstar-coolspin-sata-iii-6gb-s-hard-drive-5900rpm-32mb-0f12117) * 4

逐个解释：

主板和处理器选择 AMD 新出的 APU 方案，一大半是由于其功耗上的优势；此平台实际运行功耗25W左右，是带 T 后缀的 Intel i3 系列的一半。虽然性能上无法与 Intel i3 相提并论，但作为一个 NAS 的处理器是超标的，至少比市面上任何成品家用 NAS 盒子都强得多。

选择平台时还遇上了一个问题，就是对比 Asus E35M1-M Pro 和 [Gigabyte GA-E350V-USB3](http://www.amazon.co.uk/Gigabyte-GA-E350N-USB3-E350N-USB3-Mini-ITX-Motherboard/dp/B004NW0KY2) 。华硕和技嘉在品牌上不相上下，这两块板价格相当，主要参数也相当接近，应当如何取舍？最终我选择了华硕这块板，基于两点理由：

- 华硕这块板自带5个 SATA 槽，比技嘉的多出一个，换句话说，不加卡的前提下，华硕这块板能多挂一个硬盘，作为 NAS 这是个大优势。
- 华硕这块板的处理器是被动散热，无风扇噪音低，长期开机这也是个优点。

至于技嘉板优势在于主频高，板子面积小，装 HTPC 这块板更好一些，但这次还是不选它了。

内存方面，现在 DDR3 内存正是论斤卖的时候，不过初始我只预计了 4GB 的配额。后来由于要使用 ZFS 文件系统，[需要至少 6GB 安装内存才能达到最佳性能]http://doc.freenas.org/index.php/Hardware_Requirements#RAM，只好多插一条。如果使用别的系统，比方 Windows ， 4GB 就足够了。须知那些成品家用 NAS 恐怕连 1GB 的都很少。

机箱没什么好说的，看个人爱好。我买了个比较大的，方便日后增加硬盘。电源不需要买太大功率，因为整机功率不高。我的标准是大厂，安静。有这两点就差不多了。

NAS 的核心部分自然是硬盘了。现在大容量硬盘的故障率有多高大家都清楚，买硬盘就是拼人品。但故障率跟品牌还是有联系的，根据[这篇文章](http://blog.backblaze.com/2011/07/20/petabytes-on-a-budget-v2-0revealing-more-secrets/)，故障率最低的大容量硬盘品牌既不是希捷，也不是西数，而是日立。这一次我入了4块 2TB 的日立盘，如果对容量要求更高，可以一次到位买8块 3TB 的日立盘，组 RAID 6 或者 RAIDz2 还能达到 18TB 的容量， RAID 10 都能有 12TB ，不过需要额外买一块 PCI 转 SATA 的卡。

### 3. 系统的选择 ###
大家应该意识到这其实是一台电脑，所以...跟那些成品 NAS 有很大的区别，可以安装任何操作系统，甚至可以在操作系统里面安装虚拟机再装另一个操作系统。从 Flexibility 上考虑，我推荐直接安装 Windows 7 或者 Windows Server 2008 。有以下两点好处：

- 不但可以做 NAS ，而且直接变身一台备机，可以用来做超多事情，甚至直接当主力机器用。这个平台跟 Atom 不同，性能强很多，大部分日常工作胜任有余。
- 软件丰富，配置简单，要 stream 什么的都有解决方案，毕竟是 Windows 。而且没有学习曲线，立即就能用。

这个方案不大好的地方包括：

- 理论上， Windows 是要买的，尽管大部分中文区用户不会真的花钱，但这其实是不对的...
- 上面一节忘了说明， APU 平台现在的主板都不带 RAID 。也就是说只能用软 RAID 。而 Windows 7 下软 RAID 方式有限，只能做 stripe 和 mirror 。Windows Server 2008 倒是可以做软 RAID 5 ，但是性能臭名远扬。四盘 stripe 的风险有多高就不必说了， Mirror 损失一半空间，我实在接受不了。 Windows 7 还可以用类似 JBOD 的技术把四块硬盘合成一个卷使用，经查证也是类似 stripe ，一块盘完蛋就全完蛋，不敢用。
- Windows 支持的文件系统数量较少， FAT 家族，NTFS ... 不支持 ZFS 等高级文件系统，这个问题单独看倒不是特别严重，但是跟上一个放一起看就很别扭，总之，如果有四块硬盘，装 Windows 的话最佳方案是完全分开使用，设置四个盘符，四个共享。如果不嫌麻烦，其实也不算太大的问题...

其实后两点有个很简单的 workaround ，就是前面说的， Windows 系统中安装虚拟机，虚拟机里面使用 FreeBSD 或者 Linux 来管理硬盘。不过其一会遇到性能上的问题，其二嘛...这未免太折腾了。

假如出于种种原因，不想装 Windows ，还有什么选择呢？ 爱折腾的人可以装个 Linux 随便整，如果你有信心这样做那么自然是高手，我就不多说了。否则，个人推荐使用 FreeNAS 这个完全基于 FreeBSD 的系统。

这个系统的好处只有三个字母: [ZFS](http://en.wikipedia.org/wiki/ZFS) . 太强大的文件系统了，天生是 NAS 的绝配。(Well... 任何系统的绝配... ) 由于我也刚接触，不敢说太多，以后有什么心得再分享。现在主要是使用它的 RAIDz 功能，把四块硬盘合成一个带冗余的 6TB 卷，通过 FreeNAS 的共享功能可以方便地在 Mac OSX 和 Windows 里面访问，也可以直接作为 Mac 上 Time Machine 的备份盘。可以定时做 Snapshot ，可以定时 做 S.M.A.R.T 检查，出现问题的话自动发邮件通知等。真正的高手也可以摈弃 FreeNAS ，直接在 原装 FreeBSD 上自行完成这些功能，我是做不到的...

update: 忘了说一点。系统盘和数据盘应当严格分开。

### 4. 总结 ###

折腾的过程也是学习的过程。在 DIY 这个 NAS 硬件的过程中我总算 catch up 了一些最新的硬件知识，这方面好久没有关注了。安装配置 FreeNAS 的过程中学习了 ZFS 这个文件系统的一些知识。当然最重要的收获还是以相对低廉的价格 (约450镑，在中国购买配件的话恐怕低于四千块人民币，仅仅是一个品牌四盘位 NAS 的空壳价格) 得到了一台配置强劲系统灵活，拥有 6TB 容量 (拜可恶的1024问题所赐，实际可用空间显示为 5.3TB T_T) 的NAS 。在此鸣谢在 twitter ，论坛和 irc 给了我大量帮助和指导的朋友们，Love you <3 :-)
