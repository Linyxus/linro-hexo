---
title: Simple Tutorial to Kotlin(1)[Hello World]
date: 2017-05-27 19:14:16
tags:
- tutorial
- kotlin
categories:
- Kotlin

---

Kotlin是一个很讨人喜欢的语言，从名字到语法到理念。这里，会写出Kotlin的HelloWorld——正如大多数教程一样。同时，Kotlin的美丽也会初露一角。

<!-- more -->

```kotlin
fun hello(str: String) {
    println(str)
}

fun main(args: Array<String>) {
    var str: String
    val A: String = "Hello"
    val B = "World"
    str = A + " " + B
    hello(str)
}
```

和C++一样，Kotlin也选择main函数作为程序的起点和入口。从这段最简单的代码之中，可以看出什么呢？

### 变量的定义

如你所见，Kotlin中定义变量有两种基本形式：

```kotlin
var variant_name[: type_name] = initial_value
val const_name[: type_name] = const_value
```

>   Note: []意为可以省略的。

这分别对应着变量与常量。常量必须赋以初值，而变量可以不赋值。如果给定初值，那么Kotlin可以进行类型判断而允许我们省略类型的指明。

Kotlin中变量的定义是Pascal式的，我想很多人可能学习的第一门语言就是Pascal，是不是感觉很亲切呢0v0Pascal的古老感配上很多现代而强大的语法，使Kotlin有种古典的美感。

### main函数

和C++类似，main函数接受字符串组为参数。值得一提的是，虽然C++中main函数的参数可以略而不写（如果没有用到参数的话），在Kotlin中，main函数的参数是必须的。

换而言之，在Kotlin中，编译器寻找的不仅是函数名为main的函数，而且要求参数列表也完全匹配。如果有如下代码：

```kotlin
fun hello(str: String) {
    println(str)
}

fun main(args: Array<String>) {
    var str: String
    val A: String = "Hello"
    val B = "World"
    str = A + " " + B
    hello(str)
}

fun main() {
    hello("Hello Main!")
}
```

编译器会选择上一个main而非下一个作为入口。也即，程序的输出是"Hello World"。

### 函数的定义

```
fun function_name({arg_list, seperated by comma}) [: return_type] {
    //main_body
}
```

### 总结

这是一段最简单的Kotlin代码，从这之中，我们已经得以一窥Kotlin的美丽。