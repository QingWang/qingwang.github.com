---
layout: post
title: 用WordPress做独立博客的注意事项 (安装篇)
categories:
- 博客心得
tags:
- blog
- wordpress
- 主题
- 安装
- 插件
- 教程
- 独立博客
- 配置
published: true
comments: true
---
那么，你[挑好了域名](http://webabie.com/how-to-become-an-independent-blogger-using-wordpress-choose-domain-name/)，[买好了主机](http://webabie.com/how-to-become-an-independent-blogger-using-wordpress-choose-host/)，下一步就该安装WordPress了，我们把从下载WordPress程序到正式开始写文章这期间的工作统称为安装，显然此过程还包含了调试，美化，优化等一系列步骤。这些都是繁琐而没有什么条理的工作，我尽量尝试着整理出一个普适性的流程来。

##### 1. 下载安装 #####

WordPress的安装是非常简单的，文档说明也很详细，我就不多嘴了。但一个一个小文件往ftp上传是很让人上火的，特别是网速不好更加郁闷。如果能用telnet或ssh登录到主机上，这一步可以简化很多。

首先登入主机，然后输入以下命令：

{% codeblock lang:bash %}
wget http://wordpress.org/latest.zip
unzip latest.zip
{% endcodeblock %}

一般来说，输入"unzip l"然后按一下tab键就自动补足了，除非目录下面还有别的l开头的文件。

这样一来程序就被直接解压到wordpress目录中了，可以把这个目录名字改成你想要的，比如blog，也可以把文件都移动到根目录下面。这样比下载到本地 -> 解压 -> 上传的程序要快很多很多。不过有个问题就是config文件还需要填入数据库资料，可以把这个文件下载回来改好了传回去，也可以直接用vi改，vi的使用方法google一下就清楚了。

以上是一点小技巧，没有shell权限的只能老老实实ftp上传了。

##### 2. 初步设置 #####

装好以后立刻进后台，有几件事要马上完成。

- **修改个人资料及密码**。这是最优先的事项。WordPress系统会给你创建一个初始密码，我估计没有人会去记那个吧。个人资料里面，如果不想显示作者为admin的话，填一个昵称，保存，然后就可以选显示昵称了。这两项是一般都要改的，至于联系方式什么的想填就填，不填也无所谓。

- **填好博客的基本信息**。在Settings的General中把博客的名字、副标题、地址等填好，时区也设定好。然后到Manage的Categories里面把缺省的分类名字改了。
- **开启[Akismet](http://akismet.com/)**。这是官方的垃圾评论过滤插件，现在垃圾评论实在太多了，不喜欢Akismet也无所谓，反正总要装一个过滤插件，[spam karma](http://unknowngenius.com/blog/wordpress/spam-karma/)也不错。我比较懒，Akismet也过得去，就不换了，启用这个插件需要一个API Key，跟着提示一步步走就是了。

- **更改永久链接形式**。不但事关SEO，也让你自己和别人看链接的时候比较愉快，看个?p=143之类的谁知道你文章写啥啊。在Settings的Permalinks下，建议使用Month and name，或者用更短的，选custom structure，填入 /%postname%/ 即可，又或者你决定以后要把所有页面都静态化以降低主机负载，那填 /%postname%.html  就行了。

- **烧制feed**。对于希望发展壮大的博客来说这是很重要的，烧制了feed可以保证你即使换了域名也不流失订阅读者，而且把大量的流量转嫁给烧制服务提供商，还能方便看到一些订阅的统计信息。现在由于最好的服务商[feedburner](http://www.feedburner.com/)被大防火墙屏蔽在外，我们别无选择，只能用[feedsky](http://www.feedsky.com/)，幸好其服务也不算太差，就是feed更新实在是慢。

以上是必须进行设置的几点，其它的诸如可否注册，第一次评论是否需要验证，是否开一个低权限帐号专用于写作等等就看你自己的选择了。

##### 3. 安装WordPress插件 #####

能用插件扩展功能的程序都是强大的，Firefox如是，WordPress亦然。但插件绝不能贪多，可装可不装的坚决不装。插件多了影响速度，而速度是最最重要的用户体验，所以插件以少而精为上。

推荐装的插件有这么几个：

- [中文WordPress工具箱](http://yanfeng.org/blog/wordpress/kit/)。这个不用说了，中文WordPress博客里，几乎人手一个。

- [WordPress Database Backup](http://www.ilfilosofo.com/blog/wp-db-backup)。平时没什么用，紧要关头能救命的插件。启动以后，稍做一下设定，每天把数据库自动备份寄到邮箱里，万一发生空间商跑路等突发事件，你的损失也不至于太大。

- [All in One SEO Pack](http://wp.uberdose.com/2007/03/24/all-in-one-seo-pack/)。这是好东西，好在傻瓜式，装上启用就行了，别的基本不用管。

- [Share This 中文](http://www.happinesz.cn/archives/328)。在每篇文章的下面加一个分享按钮，装上以后不要让WordPress自动升级，升级以后是英文版，少了中文的分享网站。

- [Wordpress Thread Comment](http://blog.2i2j.com/plugins/wordpress-thread-comment)。可以回复评论，嵌套回复，而且可选ajax方式。

- [WP-PageNavi](http://lesterchan.net/portfolio/programming.php)。加入一个页面导航，一般放在Footer里，不但方便用户，也方便了搜索引擎。

- [No Self Pings](http://blogwaffe.com/2006/10/04/421/)。经常会在新帖里引用自己以前的文章，要是每引用一次都ping一下自己那就乱了。这个插件可以保持版面的整洁。

- [Google XML Sitemaps](http://www.arnebrachhold.de/redir/sitemap-home/)。让Google蜘蛛更方便。

对于插件，每个人的需要都不一样，不能照搬别人的列表。如果是新手，我建议只装最基本的，以后有需要可以慢慢添加，不要一下装太多。

上面提到的页面静态化，推荐[cos-html-cache](http://www.storyday.com/html/y2007/1213_cos-html-cache-2.html)。是时下最好的静态化插件。

##### 4. 安装配置WordPress主题 #####

主题是WordPress最吸引人的特色之一，网络有数十万计的免费主题提供下载。主题比插件更加个人化，完全凭个人喜好。我的喜好是简洁清爽型。关于主题的选择没有什么可以说的，萝卜青菜各有所爱，所以只说几点大方向性的东西。

- 注意主题与内容的搭配。比方女性的内容可以选粉色、可爱型的主题；数码产品内容可以选个iPhone的主题等等。只要协调就好。

- 主题要在主流浏览器下通过测试。一般来说IE6是最麻烦的，所以如果能通过Firefox和IE6这两者，基本上就问题不大了。每当修改了代码、加上或去掉一个widget，都要测试一下。

- 侧栏精简为佳。不需要把所有widget都用上，只放需要的。

- 把你最想展示的内容放在侧栏顶部。要让访客一眼就看见你的得意之作，不要放在翻页以后，访问者不一定会往下拉的。

- 几点必须更改的模板代码。Google analytics的代码要加在Footer里，[CC许可协议](http://creativecommons.org/licenses/by-nc-sa/3.0/)也要加在合适的位置，所有feed地址都要改成烧制的地址，另外在widget里面加上订阅图标，放在显眼的地方。

- 如果使用免费主题，尽量保留页脚的链接。做人要有点品，非要去掉可以联系作者征得同意。

如果希望自己的博客主题独一无二，有两条路：请人定做或者自己动手做。请人定做要花钱，自己动手花时间。如果想自己动手，网上也有很多教程可以参考，可以看看[Denis翻译的这一套](http://fairyfish.net/2007/06/04/so-you-want-to-create-wordpress-themes-huh/)，也可以参考[zEUS原创的教程(未完结)](http://zeuscn.net/archives/2008/04/21/diy-wordpress-themes-intro/)。WordPress官网上也有[一些说明](http://codex.wordpress.org/Theme_Development)。我建议如果有时间有精力有兴趣的话，自己动手做一个独一无二的主题吧。

另外我发现，在总体的设计方面，国内和国外的主题水平相当，但细节处理方面国外的主题普遍要好一些。个人看法，仅供参考。

主题配置好以后，应该说我们的安装配置就基本完成了。直接开博？当然没有问题，不过如果再做一些准备工作，会更加的方便。下一次我们再讨论最后的工序。
