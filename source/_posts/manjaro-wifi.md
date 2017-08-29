---
title: Manjaro中USB WiFi遇坑笔记
date: 2017-08-29 21:59:54
tags:
- manjaro
- tutorial
- memo
categories:
- 0

---

{% asset_img 1.png %}

## 问题重述

这几天实在看不惯台式机上的Ubuntu，总是琢磨着要装个Arch。但是临近开学，时间实在不多，于是乎选择了Manjaro，又有Arch强大的pacman和AUR，也几乎不用折腾。

<!-- more -->

但由于主板不带无线网卡，使用的USB WiFi的驱动总是个大问题。Ubuntu Gnome自带了很多USB WiFi驱动，而我用的chipset是RTL8192eu，刚好也包括在内，所以当初装Ubuntu时的连WiFi经历并不坎坷。但自带的驱动为了compability，对8192eu的支持只能算马马虎虎，WIFi信号比较弱且频繁出现连不上的状况，但一旦连上了就较为稳定，所以几个月来一直将就着。

**在LiveCD里装Manjaro**，我发现系统自动识别了我的网卡，然而信号极差，并且连接成功率为0。无奈先用手机共享网络，凑活着配置。

## 解决方案

在~~gayhub~~里面搜索了一下8192eu，随便挑了一下[这个](https://github.com/masterzorag/RTL8192EU-linux)。上一个commit就在十几天前，应该靠谱一些。并且Readme里面讲支持4.12，而Manjaro为4.9，大概是向下兼容的吧。

安装过程非常smooth，`make && make install`一会儿就完事儿了。然而并没有什么效果。显然是Manjaro自带的网卡module占着hardware，`lsmod | grep rtl`发现果然有个叫做`rtl8xxxu`的在安稳的工作。

老铁真是厉害，8xxxu系列通吃优化的好才怪呢\_(:зゝ∠)\_。查阅了一下wiki，把这个模块blacklist了，重启一下，发现WiFi信号满格，连接成功！0v0信号强度和稳定性比之前Ubuntu的好多了。

{% asset_img 0.png %}

我们自己编译安装的module名字就叫做8192eu，性能极佳，在这里感谢这位大佬啊！！