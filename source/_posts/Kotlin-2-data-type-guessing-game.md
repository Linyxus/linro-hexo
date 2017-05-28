---
title: Simple Tutorial to Kotlin(2)[数据类型与猜数游戏]
date: 2017-05-28 20:33:06
tags:
- kotlin
- tutorial
categories:
- Kotlin

---

这里，我们会接触Kotlin的基本数据类型，并着手写一个猜数游戏。这应该是最简单的一个游戏了，很多编程教程都把它作为例子。学完今天的例子，我们就有能力写出这样一个游戏出来啦。

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

## 基本数据类型

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

### 字面常量

-   十进制:

     `123`

    -   长整数要加上 `L`: `123L`

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