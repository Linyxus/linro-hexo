---
title: Simple Tutorial to Kotlin(-1)[代码风格]
date: 2017-05-27 21:52:49
tags:
- kotlin
- tutorial
categories:
- Kotlin

---

这篇文章会展示Kotlin的代码风格，可以说，Kotlin的代码风格很漂亮，比Java好看多了，更是毋庸置疑的超越了C++这样的语言。

<!-- more -->

```kotlin
package Idioms
data class Person(val name: String, val age: Int)
fun Int.isNegative(): Boolean = if (this < 0) true else false
fun relu(x: Int) = when {
    x.isNegative() -> 0
    else -> x
}
fun main(args: Array<String>) {
    //#1 Data Class
    val p1 = Person(name = "Hello", age = 100)
    val p2 = Person(name = "World", age = 101)
    println("Person1 HashCode: ${p1.hashCode()}")
    println("Person2 HashCode: ${p2.hashCode()}")
    println("Person1 To String: ${p1.toString()}")
    //#2 Lambda
    val list = listOf<Int>(1, 2, 3, 4, 5)
    val even = list.filter { it % 2 == 0 }
    println(even)
    //#3 If
    val a = 10
    val s = if (a == 10) "a = ${a} is 10" else "a = ${a} is not 10"
    //#4 When
    val x = -10
    println("relu($x) = ${relu(x)}")
    //#5 Function Operating
    val f1: (Int) -> (Int) = { x -> x+1 }
    val f2: (Int) -> (Int) = { x -> x * 2 }
    val compose: ((Int) -> (Int), (Int) -> (Int)) -> ((Int) -> (Int)) = {f, g -> {x -> f(g(x))}}
    val f3 = compose(f1, f2)
    println("f1(f2(100)) = f3(100) = ${f3(100)}")
}
```

运行这段代码的输出：

```
Person1 HashCode: -2137068046
Person2 HashCode: -1698217165
Person1 To String: Person(name=Hello, age=100)
[2, 4]
relu(-10) = 0
f1(f2(100)) = f3(100) = 201
```

下面，会对这些代码进行一些简单的概述。

## Idioms

### Data Class

Data Class语法的目的是让我们方便快捷地创建储存数据的类型。一个编译器创建的Data Class将拥有如下成员函数：

| Function Name   | Description                              |
| --------------- | ---------------------------------------- |
| getter          | -                                        |
| setter          | for vars if needed                       |
| equals          | -                                        |
| hashCode        | -                                        |
| toString        | format example: `Person(name=Hello, age=100)` |
| copy            | -                                        |
| component1, ... | for destructuring declarations, created for all data members |

### Lambda

`{it % 2 == 0}`创建了一个Lambda，传递给`filter`函数来挑选list中的对象。

没有括号是因为函数参数列表中的最后一个参数若是Lamda，则可以写在括号之外。而这里只有一个参数，故括号也省略了。

### If expression

在Kotlin中，if有两个作用。

第一种，和C++中一样，作为条件分支语句，定义着程序的结构。

而第二种，就如例子中的一样，充当着C++中`condition ? choice_1 : choice_2`的作用。这样，`? :`这样难读丑陋的运算符就变得elegant多了。

### When

Kotlin之中的when与C-like语言中的switch类似——只不过更加强大。`when`也可作为expression使用。而且`when`的使用没有C-like语言中类型的严格限制，形式更加多样，从`Range`，`is`以及返回布尔值的函数都可以放在`->`的左边。

在强大的同时，`when`的结构也看起来很优美，不是吗？

### Function

在Kotlin中，处理函数是极为方便的。例子中，`compose`函数接受两个函数，并返回一个函数。`compose`函数是一个Lambda函数，定义的方式非常漂亮。

事实上，如果不用Lambda，`compose`必须如此定义：

```kotlin
fun compose(f: (Int) -> Int, g: (Int) -> Int) : (Int) -> Int {
    fun result(x: Int): Int {
        return f(g(x))
    }
    return ::result
}
```

是不是冗长多了？

## 总结

Kotlin的语言风格通过这几个例子已经展露出来：高效，简约。用Kotlin可以写出漂亮，可读性高的代码。而且，Kotlin的美丽绝对不止于此。