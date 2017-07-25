---
title: Linux终端中使用ProxyChains4通过代理上网
date: 2017-07-24 15:13:58
tags:
- proxychains4
- tutorial
categories:
- 0
---

## 关于ProxyChains4

相信很多同学都为了在终端下使用代理~~科学上网~~而烦恼过，export完了环境变量却又没有作用，而各种代理工具全局的设置又极为麻烦。ProxyChains4是我所知配置最快使用也方便的让终端通过代理上网的方法。例如：

```shell
proxychains4 curl ip.gs
```

没错，配置完之后，只要在你要执行的命令前加上`proxychains4`即可。虽然这名字有点长但(~~打多了就快了~~)强大的zsh不是有alias功能嘛？

<!-- more -->

> 2017.7.25 更新
>
> 简单说说增加alias的方法
>
> 打开`~/.zshrc`
>
> 在最后加上：`alias pc='proxychains4'`
>
> 然后就可以`pc curl ip.gs`啦
>
> 0v0然而看起来不咋舒服。

## 配置

### 环境要求

- Linux~~(废话)~~
- gcc
- git
- make

### 命令

```shell
git clone https://github.com/rofl0r/proxychains-ng.git
cd proxychains-ng
./configure --prefix=/usr --sysconfdir=/etc
make
sudo make install
sudo make install-config
```

至此，proxychains4安装完毕，但我们还要做一些小小的配置。

配置文件在`/etc/proxychains.conf`

这里仅给出~~ss(敏感词各位同学自行忽略吧)~~的配置：

在配置文件里找到`[ProxyList]`，然后

```ini
[ProxyList]
socks5 127.0.0.1 1080
```

大功告成啦！

![img](http://ok30v00pz.bkt.clouddn.com/o_1blpo1eif1jo8umtj6qdscpifa.png)

嗯，差不多就是这样。