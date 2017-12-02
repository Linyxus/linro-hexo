---
title: 用Spacemacs+CIDER写Clojure
date: 2017-10-20 16:42:04
tags:
- tutorial
- clojure
- helloworld
categories:
- Clojure
---

## Introduction

因为最近在玩Clojure，但是感觉Intellij+Cursive并不太顺手，具体表现在鬼畜的Paredit（Cursive给的应该也算是Paredit高仿吧）快捷键以及奇奇怪怪的问题。（重要原因还是因为Intellij长得不够好看）

于是开始玩Spacemacs+CIDER来写Clojure，感觉代码效率++++++++++++++++++++++

## Features

{% asset_img 1.png %}

(Tips: 图中是我正在写的一个Web Game0v0 [Space Game-0.1.0-demo](https://i.linyxus.xyz/0.1.0-demo))

<!-- more -->

### Inner REPL

Meta+Return+"就可以打开REPL了（ClojureScript），Ctrl+C+E可以快速Eval Expression，测试什么的简直比Jupyter还容易。

{% asset_img 0.png %}

{% asset_img 2.png %}

### Paredit

这就老生常谈了，拯救程序员于括号堆里的神器233333

可以加个hook自动在cider-mode启动的时候开启：

```lisp
(add-hook 'cider-mode-hook paredit-mode)
(add-hook 'cider-repl-mode-hook paredit-mode)
```

