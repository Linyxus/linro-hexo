---
title: Kruskal
date: 2017-03-04 20:16:17
tags:
- C++
- 图论
categories:
- Noip
---
Kruskal
## 用途
Kruskal算法用于求无向图的最小生成树。时间复杂度为O(nlogn)。
## 基本思想
`贪心法`
## 原理
### 步骤
 - 将所有边按权值大小排序
 - 用一个边集E表示选择了的边。清空这个边集。
 - 按照权值从小到大的顺序依次考虑每条边。
  - 若将这条边放入E中，不产生环，就将这条边放入E中。
  - 反之，不放入。
 - 考虑完每一条边后，E就是所求的最小生成树。
<!--more-->
### 证明
为什么若存在环，就不选择这条边呢？

考虑一个**存在环**的生成树V。在这个环中，若去掉一条权值最大的边，那么这个环包含的节点仍然是连通的，且如此得到的生成树V1不会比原生成树V差。
## 实现
### 难点
那么，怎样判断某条边加入后是否会产生环呢？***只要这条边连接的两个顶点u,v连通***，那么这条边的加入就会产生环。因为u,v之间本来就有一条通路，加入该边之后将产生两条，也就形成环了。

怎样判断两条边是否连通呢？

使用连通分量不易于合并，有一种方便高效的方法叫做`并查集`。
### 并查集
#### 原理
![f_4525.jpg](http://linfile.xyz/data/vip_data/f_4525.jpg)

并查集将顶点以树的方式组织。所有连通的顶点形成一棵树，这棵树的树根称为这组顶点的`代表元`。判断两个顶点是否连通只要分别找到他们的树根（也就是代表元），比较是否相同。合并的方法也很简单，只要将某一组连通顶点的代表元的父亲设为另一组连通顶点的代表元即可。

![f_6234.jpg](http://linfile.xyz/data/vip_data/f_6234.jpg)
#### 实现
##### 添加
递归找到树根（代表元）。
```C++
//初始化时将所有p[u]设为u自身
int find(int u) { return p[u]==u? u : find(p[u]); }
```
但是有一个问题，如果树退化成一条链，那么每次寻找都要遍历一次树，速度很慢。

解决方法很简单，每次找到树根之后把u直接连接到树根即可。但代码相应复杂了一些。
```C++
int find(u)
{
    int v = p[u];
    while (v != p[v]) {
        v = p[v];
    }
    p[u] = v;
    return v;
}
```
![f_9506.jpg](http://linfile.xyz/data/vip_data/f_9506.jpg)

##### 合并
合并的方法很简单。
```C++
//合并u,v所在的树
int pu = find(u);
int pv = find(v);
p[pu] = pv; //p[pv] = pu;也一样
```
### 代码
解决了上面的难点，终于可以写出代码了。

```C++
int eu[maxe], ev[maxe], ew[maxe]; // 表示边连接的两个顶点以及权值
int que[maxe]; //排序
int sel[maxe]; //是否选择了这条边
int p[maxn]; //并查集
int n; //顶点数
int e; //边数

void kruskal()
{
    memset(sel, 0, sizeof sel); //清空选择的边集
    for (int i = 0; i < n; i++) {
        p[i] = i; //初始化p数组
    }
    for (int i = 0; i < e; i++) {
        que[i] = i; //主要是为了方便排序
    }
    sort(que, que + e, [](int l, int r) { return ew[que[l]] < ew[que[r]]; });
    for (int i = 0; i < e; i++) {
        int k = que[i];
        int uroot = find(eu[k]);
        int vroot = find(ev[k]);
        if (uroot != vroot) {
            sel[k] = 1;
            p[uroot] = vroot;
        }
    }
}
```
## 总结
Kruskal的时间复杂度主要制约在于排序。由于使用了并查集，连通的判断与合并是非常快的，几乎可以看做常数时间。
