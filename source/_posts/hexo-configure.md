---
title: Hexo备份的转移
date: 2017-04-14 19:33:00
tags:
- hexo
- test
categories:
- 0

---

Hexo的源码可以备份到Github上，事实上，当你执行好`hexo init`的时候，hexo已经为你创建了一个.gitignore。

备份很简单，但我在还原的时候遇到了一些小问题。

<!-- more -->

## Clone

第一步，当然是克隆你的仓库到本地。

```shell
git clone {url}
```

## npm配置

执行

```shell
npm install
```

当然，没有安装hexo的别忘了：

```shell
npm install -g hexo-cli
```

**npm的版本问题也是需要留意的。**有时候需要对包进行降级。

## Hexo配置

按理说上面的事情做完就可以正常使用了，但是我踩了个坑。

因为我用的Theme是**从Github上Clone下来的**。所以我的仓库中这个文件夹是一个子仓库。所以别忘了把Theme也Clone下来。其实当初删了.git就啥事都没了，但我忘了（摊手）。

如果忘记了这一点就会出现错误：

```shell
WARN No Layout: index.html
```





