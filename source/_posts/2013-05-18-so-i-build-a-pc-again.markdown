---
layout: post
title: "So I built a PC again"
date: 2013-05-18 10:10
comments: true
categories:
- Tutorial
tags:
- diy
---
Building a PC is not that big a deal in 2013. And admittedly, it is much easier than any other type of DIY. Assembling LEGO blocks arguably has more fun, too. The reason for building a PC today, ultimately, breaks down to squeezing horse power out of a (usually limited) budget.

Now the [WinSAT][1] is not really the most accurate benchmark in the world. But I do not know which one is anyway and this one is right there. So please put up with me.

![WinSAT result][2]

Doesn't look too shabby, does it?

However I am going to show a fuller picture (pun intended) here.

<!-- more -->

![Full screenshot of the WinSAT result][3]

Well it certainly surprised me. I would not expect a virtual machine produce a result like this. The latest [Parallels Desktop][4] probably contributed a large portion. But I was still impressed by the raw power it shown, considering the relatively small amount of money spent on it.

### Principles ###

PC-building is all about requirements and balance. Here are a few principles I follow:

- Before any purchasing, think it through and list all the needs.
- Order the components that *just* or *a little bit overly* meet the needs. For anything better, the infamous [marginal utility theory][5] will kick in right away.
- Interaction before power. (If you can have either a more powerful processor or a more comfortable keyboard, go for the keyboard.)
- Do not consider "later upgrade". It is extremely rare. Most of the time you will just build or buy a new one.

### My requirements ###

2 years ago I bought a Macbook Air. Needless to say, It has given me the best experience. And it still does. However, while the processor is still good enough, the 4GB on-chip memory begins to be in the way. I hesitated to migrate to [Vagrant][6] for this very reason. [RubyMine][7] is not acting its best on this nice little laptop either. 

Another major drawback is, of course, the 128GB SSD. My iTunes library and Aperture library are both larger than this number by a magnitude. Heck even my Calibre library alone will easily fill half of it. Freeing up space now becomes a weekly job which I hate.

Lack of real graphics card is also a minor problem. I seldom play games nowadays but may still want to try out some interesting new games from time to time. FPS and RTS are not for me but at least the machine should be able to run the latest [Civilization][8] smoothly.

So basically, my needs are outlined as such:

- Usage: app/web developing, casual to mid-core gaming, photo/video processing.
- Reasonable computing speed. General development tasks do not require a very powerful processor. But I do not want the IDEs (RubyMine, Xcode etc.) lag. Plus photo and video processing relies heavily on the processor.
- As large memory as it can be. Memory is crucial for a lot of things. It is certainly more important to a developer than the processor is
- A mid-core graphics card.
- OS X compatible. Obviously...

The last need eliminated a lot of choices, which is a good thing. Many people said to me that a "Hackintosh" or a "CustoMac" can not deliver a perfect user experience like a real Mac can do. Well, I can not argue with that if the physical appearance of the machine is counted in. But other than that. I do not find any meaningful difference, really. The crucial point here is choosing the right components so that no extra effects are needed to make it work.

### The processor ###

![Intel Xeon E3-1240 v2][9]

That's right, a Xeon. [E3-1240 v2][10] is the equivalent Xeon processor to [Core i7-3770][11] but does not have the integrated HD 4000 graphics processor. It is the logical choice since a separated graphics card will be used. And it is cooler (in both ways).

I bought it from Taobao for about £145 and have someone send it to me along with many other groceries. In the UK the price will be much higher. 

### The motherboard ###

![Gigabyte GA-Z77-DS3H][14]

This is the tricky component. For first time CustoMac builders, do not be clever, only choose from [this list][13].

My choice was the GA-Z77-DS3H because it is cheap and has 4 memory slots. 

### The memory ###

Not much to tell. In 2011 I bought some extra memory when [building the NAS][12]. Now it is of use. 4GB * 4 == 16GB

If a Xeon processor is used, there is no point to buy 1600 memory since Xeon can not be overclocked.

### The graphics card ###

![Gigabyte NVIDIA GTX650Ti][15]

Again there were not many choices. A native support graphics card was necessary. I could not install the operating system with the integrated graphics processor first, then install the driver of the real graphics card. Because there was no integrated graphics processor. 

I followed [the instruction][16] and picked a GTX 650 Ti. It is a 6xx, it is quiet and power efficient, it also has a vga port. That's right. I have a monitor with vga port only. I do plan to retire it later. Now it is being used as a secondary monitor in my set up.

### The HDD ###

I have 2TB * 4 Hitachi HDDs but apparently another system HDD is needed. I went for the Samsung 840 500GB. I knew the 840 Pro series is generally considered faster and of better quality. But capacity is more important to me. It came with 3 years guarantee anyway.

I chose Samsung because they are doing a [cash back scheme][17]. It is still going on and will expire on 28th May, 2013.

### The case and the PSU ###

Just an old normal Cooler Master case. A [Cooler Master Silent Pro M2 620][18] was bought for the machine. It was a rushed decision. You can (and probably should) get a better PSU. 

### The monitors, keyboards, mice etc ###

![Input devices][19]

All old stuff. Two monitors and a few input devices.

### The bill ###

Always the most exciting part, hah.

Paid this time:

Processor £145 + Motherboard £70 + Graphics card £105 + Processor cooler £20 + HDD £220 + PSU £80 == £640

Alternatively, this money can buy a [nice little Mac mini with Core i7, integrated HD 4000 graphics processor, 4GB memory and 1TB Winchester HDD][20]. The closest Mac mini setup (fastest processor, 16GB memory, 256GB SSD, etc.) costs a whopping £1239 and does not have a proper graphics card. 

I am quite satisfied with this outcome. 

[1]: http://en.wikipedia.org/wiki/Windows_System_Assessment_Tool "Windows System Assessment Tool"
[2]: http://farm9.staticflickr.com/8131/8747262751_c0f00eddab_o.jpg
[3]: http://farm8.staticflickr.com/7314/8747262771_2cb373f543_c.jpg
[4]: http://www.parallels.com/products/desktop/ "Parallels Desktop 8"
[5]: http://en.wikipedia.org/wiki/Marginal_utility "Marginal Utility"
[6]: http://www.vagrantup.com/ "Vagrate"
[7]: http://www.jetbrains.com/ruby/ "RubyMine"
[8]: http://www.civilization.com/ "Sid Meier's Civilization"
[9]: http://farm8.staticflickr.com/7306/8747589959_7d62c4e59e_c.jpg
[10]: http://ark.intel.com/products/65730/
[11]: http://ark.intel.com/products/65719/
[12]: http://webabie.com/diy-nas-for-home-use/
[13]: http://www.tonymacx86.com/351-building-customac-buyer-s-guide-may-2013.html#motherboards
[14]: http://farm9.staticflickr.com/8539/8747697439_6199951dea_c.jpg
[15]: http://farm8.staticflickr.com/7284/8748878926_fc51d3d4d5_c.jpg
[16]: http://www.tonymacx86.com/351-building-customac-buyer-s-guide-may-2013.html#gfx_cards
[17]: http://www.samsung.com/uk/ssdcashback/
[18]: http://www.coolermaster-usa.com/product.php?product_id=3096
[19]: http://farm9.staticflickr.com/8533/8747970701_8da84a05f0_c.jpg
[20]: http://store.apple.com/uk/browse/home/shop_mac/family/mac_mini