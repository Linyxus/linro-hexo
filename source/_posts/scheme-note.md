---
title: Scheme学习笔记
date: 2018-02-22 19:37:37
tags:
- scheme
- note
categories:
- Scheme
---

## `let`向`lambda`的转换

在Scheme中，`let`往往是这样实现的：

```scheme
(let ([var1 val1]
      [var2 val2]
     ...)
e1 e2 ...)
```

可以被转换到：

```scheme
((lambda (var1 var2 ...) e1 e2 ...) val1 val2 ...)
```

## `lambda`的几种不同参数形式

### 1

```scheme
(lambda (a1 a2 ...) ...)
```

固定参数个数，按位置分配给形参。

### 2

```scheme
(lambda x ...)
```

所有参数作为列表分配给`x`。

### 3

```scheme
(lambda (a1 a2 . x) ...)
```

上两种方式的结合，先分配前几个形参，剩下的参数作为列表分配给`x`。

<!-- more  -->