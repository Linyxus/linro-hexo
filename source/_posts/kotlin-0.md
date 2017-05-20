---
title: Simple Tutorial to Kotlin(0)
date: 2017-05-20 18:52:38
tags:
- hello
- tutorial
- test
categories:
- Kotlin
---

这篇文章会简要的介绍Kotlin IDE的配置*(Intellij IDEA)*。

如你所知，在今年的Google IO，Kotlin被宣布作为Android的官方开发语言。

<!--more-->

![“io 2017”的图片搜索结果](data:image/png;base64,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)

![img](https://thetechportal.com/wp-content/uploads/2017/05/kotline-google-990x495.jpg)

事实上，Kotlin本身基于JVM，并且有idea的强力支持，而Android Studio与idea有着同根同源的关系，Kotlin非常适合作为Android App的开发语言。

## Kotlin IDE Configuration(Intellij IDEA)

[Official Document](https://kotlinlang.org/docs/tutorials/getting-started.html)

### 安装Intellij IDEA(Linux)

1. [Download](https://www.jetbrains.com/idea/download/index.html)
2. 解压
3. `./{Unpack directory}/bin/idea.sh`

### 安装JDK(Linux)

一般来说，Linux上JavaSE已经安装好了，但是jdk不一定安装了。

在Ubuntu上安装很简单，Terminal：

```shell
sudo apt-get install openjdk-8-jdk
```

测试一下是否安装成功：

```shell
javac
```

### Start with Kotlin

新建一个Kotlin Project。

![img](http://ok30v00pz.bkt.clouddn.com/o_1bgioqqjr1rfc1eag6891gog1av7a.png)

在src中添加文件

![o_1bgis087m15pb9im1e6o1b9g179ha.png](http://ok30v00pz.bkt.clouddn.com/o_1bgis087m15pb9im1e6o1b9g179ha.png)

运行

![run](http://ok30v00pz.bkt.clouddn.com/o_1bgis42lv1rl0etqb1rr8rogsa.png)

顺带一提，这个K标志只有在你写了main函数之后才会出现。
