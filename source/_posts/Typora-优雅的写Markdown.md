---
title: Typora-优雅的写Markdown
date: 2017-04-02 23:40:43
tags:
- hello
categories:
- 0

---

# What's Typora

[Typera.io](https://typora.io/)

Typora是一个简约，强大，干净并且（划重点）漂亮的MD编辑器。

官网很elegant，编辑器也很elegant。很干净也很强大。

用惯Atom写MD的我立刻被惊艳到了，并且再也用不惯Atom了。Markdown就该这么写嘛0v0。

<!-- more -->

## 强大之处

### Math Block

这个实时预览LaTeX的功能真心想点一百个赞。 官网上貌似说是用[MathJax](https://www.mathjax.org/)渲染的。

只要打`$$`加回车，就进入了Math Block模式。来个栗子：
$$
\sum_{k=1}^{n} \frac {x_k^2} {k}
$$
![o_1bcnlp7jf4op1iii8m713nk1p5sa.png](http://ok30v00pz.bkt.clouddn.com/o_1bcnlp7jf4op1iii8m713nk1p5sa.png)

写完敲Ctrl+Enter，完工。

还有更加贴心的**错误提示**功能：

![o_1bcnls1hh10mn3lqug4alpmha.png](http://ok30v00pz.bkt.clouddn.com/o_1bcnls1hh10mn3lqug4alpmha.png)

在曾经漫长的用Atom写Markdown的岁月里，这些方便都是我不曾体验到的。

### 贴心

不知道你们有没有留意，官网上的功能介绍里，还有YAML Front Matters这一项。这简直就是为Hexo设计的。（当然也可能有别的只是我不知道）这样一来，Hexo的Post前面的yaml就毫无违和感了。

### Import

Typora还基于pandoc提供了导入功能，虽然用命令行也不是什么难事，但是给了个GUI总归顺手不少是不是。

## 安装

Typora很强，但是安装真的不简单。准确的说，在天朝把deb下下来不简单。

Win上基本没什么难度，在Ubuntu安装Typora成功的一小时前我已经在Win上感受了一下。Mac用不起，没发言资格。

用官网上的安装教程是完全行不通的。

```shell
# optional, but recommended
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys BA300B7755AFCFAE
# add Typora's repository
sudo add-apt-repository 'deb https://typora.io ./linux/'
sudo apt-get update
# install typora
sudo apt-get install typora
```

无论是否挂VPN，在我的尝试中，始终没有成功的安装。**下载实在太慢**。

只能下deb手动装了呗，也不是啥难事。于是切到Windows试图用迅雷下载，看看有没有镜像。

结果并不尽如人意，总共三个源，下载速度徘徊在10k/s，慢得让人发指。百度和Google了一下别人上传的deb包，发现只有CSDN有比较新版本的，但很不巧，~~宛若智障~~的CSDN服务器在这个时候维护了。没办法，硬着头皮下吧。

等待到11点，再看看，发现进度到了99.9%。盯着进度条看了数分钟之后，迅雷遗憾的告诉我，下载失败。

我认为当时我的心理阴影面积是求不出来的。$ S\to\infty $ 。

所幸，~~宛若智障~~的CSDN服务器在这个时候维护完了。所以我现在才有用Typora在这写文章的份。

### 下载地址

辛辛苦苦下下来的deb传到了自己的七牛云上，下载速度还可以，100k/s左右，放这儿纪念一下。

[Download](http://ok30v00pz.bkt.clouddn.com/o_1bcnfl24l1ippfe3o8t1v3a13fta.deb)

## 不足之处

有时候会出现多个光标，可能文本编辑器完全是自己重写的，有一些bug，但是无伤大雅了。

