---
title: Simple Tutorial to Kotlin(3)[if,when,for,while]
date: 2017-05-30 13:51:56
tags:
- kotlin
- tutorial
categories:
- Kotlin

---

流程控制在一段代码中起着重要的作用。举个简单的例子，如果你要写一个`max`函数，返回两个参数之间较大的那一个，就要用到`if`进行判断。又比如你要求出1+2+...+n的值，那么`for`或者`while`将使这件事变得简单（否则就不可避免地需要使用递归）。

<!-- more -->

## if和when

`if`和`when`都是条件判断语句，在上一篇中的游戏中，已经大量的使用了`if`语句，并用了一次`when`。

### if

`if`具有如下基本结构：

```kotlin
if (...) {
    ...
} else {
    ...
}
```

在大体上与C++类似，在小括号中写上一个表达式，如果求出表达式的值为`true`，则执行第一个`{}`中的语句，否则，执行第二个中的。

为了得到引言中说的`max`函数，我们可以这么写：

```kotlin
fun max(a: Int, b: Int): Int {
    if (a > b)
        return a
    else
        return b
}
```

#### if-expression

之前已经提到，Kotlin中的`if`也可以返回一个值，这一特性称为if-expression。

比如上面的`max`函数也可写成：

```kotlin
fun max(a: Int, b: Int): Int { 
    return if (a > b) a else b
}
```

使用if-expression要求：

1. 具有else分支
2. 两个代码块的最后一行为一个表达式

由于只要求最后一行是表达式，所以这样的代码也是可以接受的：

```kotlin
fun max(a: Int, b: Int): Int {
    val v = if (a > b) {
        println("a is bigger!")
        a
    } else {
        println("b is bigger! or a equals to b")
        b
    }
}
```

当然，正如之前提到的，如果最后一行是（看似）一个不返回值的函数，那么一个默认的`Unit`会被返回。

### when

`when`可以看作是多个`if`的简写，用于进行多种情况下的选择。

#### 形式1

```kotlin
when (x) {
    1 -> println("x == 1")
    in 2..3 -> println("2..3")
    4, 5 -> println("4 or 5")
    is String -> println("is String!")
    else -> {
        println("I dont know!")
    }
}
```

当`()`中有值的时候，上面的例子涵盖了基本全部的形式。只要有`else`，`when`也可作为expression。

#### 形式2

```kotlin
when {
    x.isOdd() -> print("x is odd")
    x.isEven() -> print("x is even")
    else -> print("x is funny")
}
```

如果没有`(...)`，那么如上的形式也是允许的，并且也可以作为expression使用。

## for和while

`for`和`while`都可以做一件重要的事情，那就是循环（或者叫做迭代）。如果没有他们，写代码将成为一件不大好玩的事情，比方说你要求一个数列中前50项的和，难不成要写出这样的代码：

```kotlin
var sum: Int = 0
sum = a[0] + a[1] + {此处省略很多(47)项} + a[49]
```

这显然有点傻，更别提若数组长度未知，那么求出数组中所有元素的和就成了不可能的事情。

但`for`和`while`可以正确的完成这件事情：

```kotlin
//#1
for (x in a)
    sum += x
```

```kotlin
//#2
var i = 0
while (i < a.size) {
    sum += a[i]
	i += 1
}
```

\#1做的事情是，将`a`中的所有元素依次赋给`x`并执行`{}`中的代码。

\#2做的事情略有不同，它通过下标得到`a`中的值，它所做的事情是：判断括号中的条件为`true` -> 执行代码 -> 判断为`true` -> 执行 -> ...(此处省略很多次判断和执行) -> 判断条件为`false`，停止。`while`中的判断善始善终，它作为`while`的开始也宣告`while`的结束。

想要用`for`完成\#2中的事情也不难，只要使用：

```kotlin
for (x in a.indices)
    sum += a[x]
```

`a.indices`返回`a`所有可能的下标（也即`0, 1, ..., a.size -1 `）。

## 求斐波那契数列

运用判断与迭代，我们可以写出求斐波那契数列的动态规划（或者说递推）代码。

```kotlin
var memo = Array<Long>(1000, { x -> if (x == 0 || x == 1) 1 else -1})
fun fibbo(n: Int): Long = if (memo[n] != -1L)
    memo[n]
else {
    val v: Long = fibbo(n - 1) + fibbo(n - 2)
    memo[n] = v
    v
}
```

> Notes:
>
> 1. `memo`被创建为一个长度为1000的数组，第一、二项为1，其他项都为-1
> 2. `fibbo`函数是一个动态规划的简单实现*(dynamic-programming with memo)*。为了避免重复计算定义`memo`作为储存，-1表示没有计算过，如果不为-1则表示计算过，直接使用结果即可。





