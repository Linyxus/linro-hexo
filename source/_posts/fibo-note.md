---
title: 从一个简单算法看算法与数学的联系
date: 2018-02-19 13:06:00
tags:
- tutorial
- fibo
- algorithm
categories:
- 0
---

## 关于斐波那契数列

相比各位都很熟悉...但这里还是简单说一下：

斐波那契数列是这样的一个数列$\{a_n\}:$
$$
对n \in N_+, a_{n+2} = a_n + a_{n+1}
$$
且有：
$$
a_1=1, a_2=1
$$
那么，根据定义，数列是这样子的：$1,1,2,3,5,8,13,21,34,55,89\cdots$

## 目标

我们希望可以写出一个算法，可以求出斐波那契数列中的任意项。

<!-- more -->

## 算法

### 朴素算法

根据斐波那契数列的定义，我们很容易可以写出这样的算法：

```scheme
(define (fibo n)
  (cond
   ((= n 1) 1)
   ((= n 2) 1)
   (else (+ (fibo (- n 1))
            (fibo (- n 2))))))
```

写起来容易，效率却太低了。可以证明，算法时间复杂度为$O(\phi^n)$，其中，$\phi$为黄金比例。在这样的时间复杂度之下，单单是求出第35项就旷日持久：

```shell
8.17s CPU time, 0.324s GC time (major), 1039330/173220 mutations (total/tracked), 111/52960 GCs (major/minor), maximum live heap: 2.07 MiB
```

### 动态规划/递推

通过动态规划/递推的路子可以很容易地把时间复杂度优化到$O(n)$，这里用递推的写法给出一个简单算法：

```scheme
(define (fibo-iter a b n)
  (cond
   ((= n 1) b)
   (else (fibo-iter (+ a b) a (- n 1)))))
(define (fibo n)
  (fibo-iter 1 1 n))
```

在$O(n)$之下，求出10000000项的时间是：

```shell
4.035s CPU time, 0.027s GC time (major), 558118/93018 mutations (total/tracked), 9/29958 GCs (major/minor), maximum live heap: 2.16 MiB
```

而求出第35项的时间是：

```
0s CPU time, 10/0 mutations (total/tracked), maximum live heap: 2.16 MiB
```

嗯…Chicken Scheme的`time`函数没有精确到毫秒级，凑活着看看吧。

### 矩阵快速幂

但是，这个算法可以进一步优化，把时间复杂度优化到$O(log n)$。

进行一些数学分析，考虑上一个算法中的$a$和$b$，事实上每一步迭代更新$a$和$b$的方式等价于：


$$
M
  \begin{bmatrix}
   a\\
   b
  \end{bmatrix}
  =
    \begin{bmatrix}
   a^{'}\\
   b^{'}
  \end{bmatrix}
$$
由于$a^{‘}=a+b$且$b^{'}=a$，可以知道
$$
M
=
  \begin{bmatrix}
   1 & 1\\
   1 & 0
  \end{bmatrix}
$$
那么，记：
$$
x_n =
  \begin{bmatrix}
   a_{n+1}\\
   a_{n}
  \end{bmatrix}
  ,
  n \in N_+
$$
要求出$a_n$，事实上只要求出
$$
x_{n} = M^{n-1} x_1
$$
这里，
$$
x_1 = 
  \begin{bmatrix}
   1\\
   1

  \end{bmatrix}
$$
那么，怎样求出$M^{n-1}$呢？数学分析就此结束，开始算法实现的考虑。虽然矩阵和实数不太一样，但是快速幂算法没有理由不能应用在矩阵$M$上，实现起来也很简单。

首先，实现一下简单的矩阵乘法，只适用于两个$4 * 4$矩阵：

```scheme
(define (matmul a1 b1 c1 d1
                a2 b2 c2 d2)
  (values (+ (* a1 a2) (* b1 c2))
          (+ (* a1 b2) (* b1 d2))
          (+ (* c1 a2) (* d1 c2))
          (+ (* c1 b2) (* d1 d2))))
```

别忘了$4*4$矩阵乘以一个二维向量的：

```scheme
(define (vecmul a b c d
                x y)
  (values (+ (* a x) (* b y))
          (+ (* c x) (* d y))))
```

然后几乎不加修改地写出快速幂，所有改变只是把乘法改成了上面两个函数：

```scheme
(define (fastexp a b c d
                 n
                 x y)
  (cond
   ((= n 0) (values x y))
   ((even? n) (let-values (((aa bb cc dd)
                            (matmul a b c d
                                    a b c d)))
                (fastexp aa bb cc dd
                         (/ n 2)
                         x y)))
   (else (let-values (((xx yy)
                       (vecmul a b c d
                               x y)))
           (fastexp a b c d
                    (- n 1)
                    xx yy)))))
```

> 因为Scheme丑陋的values语法，这段代码看起来不太elegant。

随后，写出最终的`fibo`算法也顺理成章毫无难度：

```scheme
(define (fibo n)
  (let-values (((a b) (fastexp 1 1 1 0
                               (- n 1)
                               1 1)))
    b))
```

正如之前提到的，这个算法的时间复杂度是$O(logn)$，那么，它计算第10000000项需要多久呢？

```shell
0s CPU time, 1860/4 mutations (total/tracked), 0/1 GCs (major/minor), maximum live heap: 2.27 MiB
```

嗯…非常优秀。

### 实数快速幂

在上面的算法中，用到了矩阵的快速幂，其实这一创新并不是必须的。数学分析不必那么快结束，下面仍有很多工作可以做。

我们要求出的是：
$$
M^{n-1}x_1,
M = 
  \begin{bmatrix}
   1 & 1\\
   1 & 0
  \end{bmatrix}
  ,
x_1=
  \begin{bmatrix}
   1\\
   1
  \end{bmatrix}
$$
先求出矩阵$M$的两个特征值以及特征向量：
$$
\lambda_1 = \frac{1-\sqrt{5}}{2},
\alpha_1=
  \begin{bmatrix}
   1\\
   \frac{-1-\sqrt{5}}{2}
  \end{bmatrix} \\
  \lambda_2 = \frac{1+\sqrt{5}}{2},
  \alpha_2=
  \begin{bmatrix}
   1\\
   \frac{-1+\sqrt{5}}{2}
  \end{bmatrix}
$$
设$x_1 = p\alpha_1 + q\alpha_2$，可以求得：
$$
M^{n-1}x_1 = M^{n-1}(p\alpha_1 + q\alpha_2) = p\lambda_1^{n-1}\alpha_1 + q\lambda_2^{n-1}\alpha_2
$$
最终就可以求出人尽皆知的斐波那契数列通项公式：
$$
a_n = \frac{\phi^n-(1-\phi)^n}{\sqrt{5}},\phi=\frac{1+\sqrt{5}}{2}
$$
下面可以用实数快速幂来计算$a_n$的值，使用快速幂算法，也是$O(logn)$的时间复杂度。不做代码实现，由于精度问题，需要取整到邻近整数。



