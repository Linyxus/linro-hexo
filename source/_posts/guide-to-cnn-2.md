---
title: 零基础理解卷积神经网络[1]
date: 2017-08-02 19:49:38
tags:
- tutorial
- cnn
categories:
- Machine Learning
---

[零基础理解卷积神经网络 - 索引页](/Machine-Learning/guide-to-cnn-index/)

本文译自[A Beginner's Guide To Understanding Convolutional Neural Networks](https://adeshpande3.github.io/adeshpande3.github.io/A-Beginner's-Guide-To-Understanding-Convolutional-Neural-Networks/)，已征得原作者同意。

## 写在前面

在这片文章中，我们要深入探讨更多关于卷积神经网络的特性。

**免责声明：**我知道这些话题中有的很复杂，甚至可以独立成章。所以为了保持简洁与综合，我会提供一些详细解释这些话题的论文链接。

## 步长*(Stride)*与边框*(Padding)*

好啦，来继续看看我们的老朋友卷积神经网络。还记得卷积核，感知域和卷积吗？很好。现在，我要告诉你还有两个参数会改变每一层的行为。在选择了卷积核大小之后，我们也要选择步长和边框。

步长决定了卷积核如何在输入图像上做卷积。在第一部分的例子中，卷积核在做卷积时每次只移动一个单位。卷积核每次移动的单位数量就是步长。在那个例子中，步长默认为1。步长一般使输出尺寸为一个整数而非一个分数。来看看这个例子，我们有一张7 × 7的图像和一个步长为1的3 × 3卷积核（简单起见，忽略深度）。这是我们早就知道的情况。

![img](https://adeshpande3.github.io/assets/Stride1.png)

多熟悉啊，是不是？猜猜当步长被设置为2时会发生什么？

![img](https://adeshpande3.github.io/assets/Stride2.png)

正如你看到的那样，感知域每次移动2个单位，并且输出尺寸也因此缩小了。