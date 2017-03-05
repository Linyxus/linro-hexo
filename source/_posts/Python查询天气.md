---
title: Python查询天气
date: 2017-03-04 20:52:04
tags:
- urllib2
- python
categories:
- Python
---
# 使用Python制作天气查询程序
## 前言
原教程地址：[Crossin](http://crossincode.com/course/lesson_list/)

这篇文章是我按照上面的教程学习后写的一篇笔记，由于我是`Python`初学者，如有不对的地方欢迎指出哦0v0

## 截图
![f_3356.png](http://linfile.xyz/data/vip_data/f_3356.png)

<!--more-->

## 原理
利用了中国天气网的API。
 - 首先，在http://m.weather.com.cn/data5/city.xml 获取各个省的编号
 ```
 01|北京,02|上海,03|天津,04|重庆,05|黑龙江,06|吉林,07|辽宁,08|内蒙古,09|河北,10|山西,11|陕西,12|山东,13|新疆,14|西藏,15|青海,16|甘肃,17|宁夏,18|河南,19|江苏,20|湖北,21|浙江,22|安徽,23|福建,24|江西,25|湖南,26|贵州,27|四川,28|广东,29|云南,30|广西,31|海南,32|香港,33|澳门,34|台湾
 ```
 这是获取到的代码
 - 解析这段代码获取各个省的编号在http://m.weather.com.cn/data5/city省编号.xml 获取该省城市代码。
 - 在http://m.weather.com.cn/data5/city城市编号.xml 获取该城市各个地区的编号
 - 最后，在http://www.weather.com.cn/data/cityinfo/地区编号.html 获取这个地区的天气信息，json格式
 ```
 {"weatherinfo":{"city":"江阴","cityid":"101190202","temp1":"3","temp2":"162","weather":"多云","img1":"n0.gif","img2":"d1.gif","ptime":"18:00"}}
 ```
 - 解析这段json，输出
## 实现
基本上没有什么难度。下面是代码：

### 代码
```Python
import urllib2
import json
prov = {}
city = {}
#Read city data
web = urllib2.urlopen("http://m.weather.com.cn/data5/city.xml")
data = web.read()
web.close()
l = data.split(",")
for s in l:
    item = s.split("|")
    prov[item[1]] = item[0]
for p in prov:
    i = prov[p]
    web = urllib2.urlopen("http://m.weather.com.cn/data5/city%s.xml" % i)
    data = web.read()
    print data
    web.close()
    l = data.split(",")
    for s in l:
        item = s.split("|")
        city[item[1]] = item[0]
for c in city:
    print "%s:%s" % (c, city[c])

#query
while True:
    print "Enter city name:"
    c = raw_input()
    i = city.get(c)
    if i == None:
        print "City not found!"
        continue
    area = { }
    web = urllib2.urlopen("http://m.weather.com.cn/data5/city%s.xml" % i)
    data = web.read()
    web.close()
    print data
    l = data.split(",")
    for s in l:
        item = s.split("|")
        area[item[1]] = item[0]
    for a in area:
        print "%s:%s" % (a, area[a])
    print "Enter area name:"
    a = raw_input()
    i = area.get(a)
    if i == None:
        print "Area not found!"
        continue
    web = urllib2.urlopen("http://m.weather.com.cn/data5/city%s.xml" % i)
    data = web.read()
    web.close()
    item = data.split("|")
    i = item[1]
    web = urllib2.urlopen("http://www.weather.com.cn/data/cityinfo/%s.html" % i)
    data = web.read()
    root = json.loads(data)
    info = root["weatherinfo"];
    temp = "%s\n%s~%s" % (info["weather"], info["temp1"], info["temp2"])
    print temp
```
### 改进
代码还有可以改进的地方
 - 比如抓取编号之后可以存储在文件中。每次打开检测是否有文件，若存在文件就直接读取文件，若不存在就抓取数据。
 - 这一段代码中有很多重复的地方，可以考虑封装成一个函数，进行代码复用。

## 总结
Python真的非常强大。
