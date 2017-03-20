---
title: 在Hexo中安装InstantClick
date: 2017-03-19 14:06:52
tags:
- other
- instantclick
categories:
- 0
---
## InstantClick
[InstantClick](http://instantclick.io/)可以使你的网页变成一个单页的网页，对页面加载速度提升很大。~~不会写React又想感受一下SinglePage速度与逼格的小伙伴么可以眼馋一下哦0v0~~

{% blockquote InstantClick http://instantclick.io/how-it-works How InstantClick Works %}
The most important thing to understand is this: InstantClick technically makes your website a single-page application; the browser doesn’t change pages anymore, InstantClick does.
{% endblockquote %}

## How to install IC in Hexo
### 最简单的方法
首先，在`themes/your-theme/`中找到scripts.ejs。
<!--more-->
我是用的主题是[MaterialFlow](https://github.com/stkevintan/hexo-theme-material-flow)，
scripts.ejs在`themes/material-flow/layouts/_partials/scripts.ejs`。当然，如果你的主题中没有scripts.ejs，只要找到一个所有页面（或者所有你想用IC的页面）Hexo都会加载的ejs。

然后在最后添加：
{% codeblock scripts.ejs lang:ejs http://instantclick.io/download InstantClick.io %}
...
<script src="http://instantclick.io/v3.1.0/instantclick.min.js" data-no-instant></script>
<script data-no-instant>InstantClick.init();</script>
{% endcodeblock %}

就可以了。但如果想把js保存在本地呢？
### 本地JS方法
首先，下载[JS File](http://instantclick.io/v3.1.0/instantclick.min.js)到`your-theme/source/js`。

Hexo为加载source文件夹中的Js提供了一个函数：
{% codeblock js() lang:ejs https://hexo.io/docs/helpers.html#js Hexo.io %}
<%- js(path, ...) %>
{% endcodeblock %}

但是无法添加`data-no-instant`标签，为了解决，只能用Hexo提供的另一个函数：
{% codeblock url_for() lang:ejs https://hexo.io/docs/helpers.html#url-for Hexo.io %}
<%- url_for(path) %>
{% endcodeblock %}

在scripts.ejs中添加：
```ejs
<script src="<%- url_for('js/instantclick.min.js') %>" data-no-instant></script>
<script data-no-instant>InstantClick.init();</script>
```

## 解决冲突
只要在之前的代码后再加入：
```JavaScript
InstantClick.on('change', function(isInitialLoad) {
  if (isInitialLoad === false) {
    if (typeof MathJax !== 'undefined') // support MathJax
      MathJax.Hub.Queue(["Typeset",MathJax.Hub]);
    if (typeof prettyPrint !== 'undefined') // support google code prettify
      prettyPrint();
    if (typeof _hmt !== 'undefined')  // support 百度统计
      _hmt.push(['_trackPageview', location.pathname + location.search]);
    if (typeof ga !== 'undefined')  // support google analytics
        ga('send', 'pageview', location.pathname + location.search);
  }
});
```
参考了开头说的那篇文章。
