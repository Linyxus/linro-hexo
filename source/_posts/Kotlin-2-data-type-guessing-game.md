---
title: Simple Tutorial to Kotlin(2)[数据类型与猜数游戏]
date: 2017-05-28 20:33:06
tags:
- kotlin
- tutorial
categories:
- Kotlin

---

这里，我们会接触Kotlin的基本数据类型，并着手写一个猜数游戏。这应该是最简单的一个游戏了，很多编程教程都把它作为例子。学完这里的内容，我们就有能力写出这样一个游戏出来啦。

<!-- more -->

```
Select difficulty:
#0 1~100
#1 1~500
#2 1~1000
0
#1 Guess:
50
Too BIG!
#2 Guess:
25
Too small!
#3 Guess:
33
Too small!
#4 Guess:
45
Too BIG!
#5 Guess:
40
Too small!
#6 Guess:
42
Got It! Total rounds:6
```

以上，是一次游戏的演示过程。

## 数据类型

### 基本数据类型

如同Java一样，Kotlin中的所有数据类型都是一个类，并且提供了一套函数，比如字符串与数之类的相互转换等等。这无疑给编码带来了巨大的便捷。

| Type   | Description |
| ------ | ----------- |
| Double | 浮点数(64)     |
| Float  | 浮点数(32)     |
| Long   | 长整数         |
| Int    | 整数          |
| Short  | 短整数         |
| Byte   | 字节          |

>   Notes:值得一提的是，赋值时必须要求类型相同，即使是把小容量类型的变量赋给大容量的。如把一个Byte赋给Long是非法的，当然把Long赋给Byte也是非法的。为了赋值，必须用形如`a = b.toLong()`的形式，显式地转换类型，然后赋值。虽然不明白这么做的意义何在，但也只能这么做了。

#### 字面常量

-   十进制:`123`

     长整数要加上 `L`: `123L`

-   十二进制: `0x0F`

-   二进制: `0b00001011`

### 字符与字符串

#### Char

字符大体上与C++中相同。也有反斜杠转义：`\t`，`\\`等等。但有一点是值得注意的，Kotlin中的`char`不可以直接当做`Int`来对待。例如，`c == 1`是会报错的。必须要显式的转化为`Int`：`c.toInt() == 1`才可以。

#### String

如同Java一样，`String`一旦赋值就无法更改。

例如，你可以这么做：

```kotlin
for (c in str) {
    println(c)
}
```

却不能这么做：

```kotlin
str[0] = '0'
```

`String`字面常量有两种定义方式：

第一种，也是最常见的：`val s = "Hello, world!\n"`。

第二种，用于定义多行文本：

```kotlin
val text = """
    for (c in "foo")
        print(c)
"""
```

但是，由于`"""..."""`中的格式是与需要定义的文本完全相同的，并不以缩进计算，所以就会导致格式的丑陋，比如：

```kotlin
if (a == b) {
    val str = """
A equals to B!
OK!
    """
}
```

怎样解决呢？

我们可以这样写代码：

```kotlin
if (a == b) {
	val str = """
		|A equals to B!
		|OK!
	""".trimMargin()
}
```

这样，我们得到的`str`是完全一样的。`trimMargin()`的作用是把每一行的`|`以及它之前的空格全部去掉。这是一个使用多行文本定义并且保留美观格式的良好解决方法。

#### 字符串中的$#### 

你们可能早已留意到，字符串有一个~~奇怪~~的符号：`$`。它的作用是求出后面表达式的值，放在字符串中。

例如：

```kotlin
val x = 1
println("x = $x")
```

然而，`$`只能作用于单变量，如果有下面的代码：

```kotlin
val x = 3.1415
println("x = $x.toInt()")
```

结果将会是：

```
x = 3.1415.toInt()
```

这不是我们想要的。正确的写法是：

```kotlin
val x = 3.1415
println("x = ${x.toInt()}")
```

结果就正确了：

```
x = 3
```

在`${}`之中，可以放上所有表达式：if，when，函数。

例如：

```kotlin
println("1 ${if (1==2) " == " else " != "} 2")
```

是不是很便捷？

> Notes:Kotlin中，所有函数都有返回值。如果没有指明返回值，Kotlin会自动使其返回一个`Unit`，表示空值。故如果在`${}`中写上了一个没有返回值的函数，则会打印出kotlin.Unit。

### Array

Array的构造方法一般有两种：

1. ```kotlin
   val array = arrayOf(1, 4, 5, 6, 8)
   ```

2. ```kotlin
   val array = Array(10, {x -> x*x})
   ```

> Notes: `{x -> x*x}`创建了一个Lambda，接受一个数`x`，返回它的平方`x*x`。

### 关于Null

在Kotlin之中，变量的值可以为`null`，用以表示错误等等。然而，一般的变量类型不允许值为`null`，想要一个变量的值为`null`必须提前告诉编译器*(Null Safety)*。

具体的办法是在数据类型后加上一个`?`。例如：

```kotlin
val x: Int = null //error
val x: Int? = null
```

可能为`null`的变量的用法：

```kotlin
if (x != null) {
    ...
}
else {
    ... //report error?
}
```

或者，可以简写为：

```kotlin
str = if (x != null) x.toString() else "x is null!"
```

还有更加简便的写法：

```kotlin
str = x?.toString ?: "x is null!"
```

此外，值得一提的是，如果一个变量被定义为可能为`null`的类型，无论它的值是否为`null`，都要使用`?.`而非`.`，例如：

```kotlin
val str: String? = "I am not NULL!"
println(str.length) //error
println(str?.length)
```

可能你想问，为什么上文中`if (x != null) x.toString() else "x is null!"`直接使用了`.`，而编译器竟然没有抗议？因为，Kotlin的编译器会**自动检测**你是否已经对`null`进行了判断，**在这里，它知道`x`是安全的，或者说，它自动把`x`转换成了不能为`null`的类型**。

关于`null`还有很多可说的，但不是现在讨论的重点，请放心，现在知道这么多已经绰绰有余了。

## 猜数游戏

为了实现这个游戏，我们要做哪些事情呢？

1. 生成随机的数字
2. 循环地询问玩家回答，直到玩家给出正确的答案
3. 每次都告诉玩家回答与答案的大小关系
4. 最好，在游戏开始前给出一个选择难度的功能

下面，我们对这些问题各个击破，最终，完整的游戏会被完成。

### 生成随机的数字

Kotlin有一个优势：由于它属于JVM平台（其实JavaScript亦有支持)，所以大多数的Java库都可以使用，事实上，Intellij甚至给出了将Java代码转换为Kotlin代码的功能。

为了生成随机数，需要使用Random库。我们`import`这个库，并撰写一个生成随机数的函数。

```kotlin
import java.util.Random
val r = Random()
fun randInt(max: Int) = r.nextInt() + 1
```

由于`nextInt`生成的随机数$x \in [0, max)$。而我们想要的是$x \in [1,max]$的数（至少我是这么想的），所以`+ 1`来满足要求。

### 读取用户的输入

我们可以采用如下形式读取玩家的输入：

```kotlin
val inp = readLine()?.toIntOrNull()
if (inp != null)
	...
else {
	...
}
```

如果，我们发现`inp`为`null`或者用户的输入是非法的（比如：要求输入数字却输出了一串字母、选择的序号不合法等等），我们可以输出错误信息，并且用`return`结束整个程序。

> Notes:`return`在Kotlin中的用法和C++一致，用来返回函数的值并且结束函数，如果后面什么也没有，那表明函数不返回值（或者说，函数返回一个表示什么都没有的Unit，这些在后面会讲到）。这里，我们用`return`结束了整个main函数，也就宣告了程序的结束。

正如函数名所表达的那样，`toIntOrNull`会尝试把`String`转换为`Int`，如果失败了，就返回`null`。

### 循环

循环是游戏的主体部分，这里直接给出游戏的主体代码并解释。

```kotlin
val value = randInt(limit)
var answer = -1
var rounds = 0
while (answer != value) {
    rounds++
    println("#$rounds Guess:")
    val inp = readLine()?.toIntOrNull()
    if (inp != null)
        answer = inp
    else {         
        println("Bad Input! Goodbye")
        return
    }
    val reply: String = when {
        answer > value -> "Too BIG!"
        answer < value -> "Too small!"
        else -> "Got It! Total rounds:$rounds"
    }
    println(reply)
}
```

这段代码之中，`value`存储着答案，`answer`存储着玩家的回答，`rounds`记录着玩家用去的总回合数。下面是一个`while`循环，直到`answer`与`value`相等时才会终止。

> Notes:`while`循环的用法在Kotlin中和C++一致，每次循环开始之前，检查括号中的条件，true则继续循环，false则停止。第一次循环之前也会检查条件。

随后，我们读取用户的输入，并决定程序的回应。在确定`reply`的过程中，使用了`when`语句，这也是Kotlin语法的优美之处，关于`when`的使用方法，下面会讲到。这里的`when`与下面这段代码是等价的：

```kotlin
val reply: String = if(answer > value) "Too BIG!" 
					else if (answer < value) "Too small!"
					else "Got It! Total rounds:$rounds"
```

上面这段代码里，使用了两个*if-expression*，他们之间有嵌套关系，你一定发现了吧0v0

### 选择难度

看似游戏已经可以写出来了。结束了吗？不不不，看看第四条，我们还有一件事情没有做：选择难度。这件事情也很好做，数组可以派上用场了。

```kotlin
val limits = arrayOf(100, 500, 1000)
val limit: Int
println("Select difficulty:")
for (i in limits.indices) {
    println("#$i 1~${limits[i]}")
}
val sel = readLine()?.toIntOrNull()
if (sel != null) {
    if (sel >= limits.size) {
        println("Out of index! Goodbye")
        return
    }
    limit = limits[sel]
}
else {
    println("Bad Input! Goodbye")
    return
}
```

`limits`保存了所有可能的猜数范围的取值，而`limit`代表了最终玩家选择的范围。下面做的事情就是决定`limit`到底是多少。可以看到，我们先列出了菜单，然后读取玩家选择的难度序号，然后进行`null`判断和合法性判断（如果菜单里没有这个序号，我们可没办法继续）。

当知道选择的序号没问题，我们执行`limit = limits[sel]`，确定`limit`的取值，进行下一步操作。

### 完整代码

我们已经做完了所有事情，一个完整的游戏已经可以写出来了，这里，我给出完整的代码：

```kotlin
package Guess
//import packages
import java.util.Random
//generate random integer
val r = Random()
fun randInt(max: Int) = r.nextInt(max) + 1
//the entry
fun main(args: Array<String>) {
    //decide the limit
    val limits = arrayOf(100, 500, 1000)
    val limit: Int
    println("Select difficulty:")
    for (i in limits.indices) {
        println("#$i 1~${limits[i]}")
    }
    val sel = readLine()?.toIntOrNull()
    if (sel != null) {
        if (sel >= limits.size) {
            println("Out of index! Goodbye")
            return
        }
        limit = limits[sel]
    }
    else {
        println("Bad Input! Goodbye")
        return
    }
    //begin the game
    val value = randInt(limit)
    var answer = -1
    var rounds = 0
    //main loop
    while (answer != value) {
        rounds++
        println("#$rounds Guess:")
        val inp = readLine()?.toIntOrNull()
        if (inp != null)
            answer = inp
        else {
            println("Bad Input! Goodbye")
            return
        }
        val reply:String = when {
            answer > value -> "Too BIG!"
            answer < value -> "Too small!"
            else -> "Got It! Total rounds:$rounds"
        }
        println(reply)
    }
}
```

> 题外话：使用二分法可以更快的找到这个数哦0v0

## 总结

猜数游戏还是挺好玩的0v0诶嘿嘿