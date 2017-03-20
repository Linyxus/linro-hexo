---
title: Play With ScrollReveal
date: 2017-03-20 20:24:07
tags:
- web
- scrollreveal
- test
categories:
- Web
---
## ScrollReveal
[ScrollReveal](https://scrollrevealjs.org/)可以快速的让你的网站有一个炫酷的滚动动画。

有多快~~♂~~ ？也就是引用一个Js和加几句代码的事情啦。
<!-- more -->
## Demo
[Hello SR](http://linyxus.xyz/code/scrollreveal/)

## How to
### 添加Js引用
```HTML
<body>
    ...
    <script src="https://unpkg.com/scrollreveal/dist/scrollreveal.min.js"></script>
</body>
```
当然，下载放到自己的服务器上也可以。
### 初始化并设置
```js
window.sr = ScrollReveal();
sr.reveal('.foo');
sr.reveal('.bar');
```
类似CSS选择器的筛选方法，需要动画的元素统一给一个class就行啦。
## 高级玩法
### 比如reset
```js
window.sr = ScrollReveal({ reset: true });
```
这样设置，元素在消失后再次出现会再有一个动画。
### More
[官方Doc](https://github.com/jlmakes/scrollreveal#22-the-starting-defaults)
```js
...
// Starting angles in degrees, will transition from these values to 0 in all axes.
rotate: { x: 0, y: 0, z: 0 },

// Starting opacity value, before transitioning to the computed opacity.
opacity: 0,

// Starting scale value, will transition from this value to 1
scale: 0.9
...
```

### 还有个不错的效果
```js
sr.reveal('.box', 50);
```
可以让符合条件的元素一个接着一个的执行动画，而不是一起。

我的主页就是这样。[Linyxus.xyz](http://linyxus.xyz/)
## 最后
还有很多自定义玩法可以探索哟0v0
