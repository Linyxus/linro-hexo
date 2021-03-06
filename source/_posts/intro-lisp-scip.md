---
title: Programming in Lisp
date: 2017-12-31 20:17:22
tags:
- lisp
- intro
categories:
- 0
---

本文译自[SICP第一章](https://mitpress.mit.edu/sicp/full-text/book/book-Z-H-9.html)，未征得原作者同意。

**正文**

我们需要有一个合适的语言用来描述过程，为此，我们选择了Lisp。正如日常想法用自然语言表达（比如法语英语或者中文）、对数量现象的描述用数学符号表达，关于过程的想法可以用Lisp来表达。Lisp在二十世纪五十年代被发明出来，它的初衷是对一种被称为递归式的特殊逻辑表达式的形式主义研究，作为一种运算模型。这门语言是由麦卡锡*(John McCathy)*构想的，并基于他的论文*"Recursive Functions of Symbolic Expressions and Their Computation by Machine"(McCathy 1960)*。

<!-- more -->

尽管它的起源是数学形式主义理论，Lisp是一门实用的编程语言。一个Lisp解释器是一种能够执行Lisp描述的过程的机器。第一个Lisp解释器由麦卡锡在学生与同事的帮助下实现的，他们在MIT电子研究实验室人工智能小组与MIT计算中心共事。Lisp的名字事实上是列表处理*(LISt Processing)*的缩写。它被设计为提供符号计算能力来解决诸如符号求导与代数表达式整合。为了这个目的，它引入了一些全新的数据类型，被称为原子*(atom)*和列表*(list)*，而这些数据类型，也让它和同时期其他编程语言有了显著的差异。

Lisp不是同心协力的努力产生的产物，相反的，他以一种实验性的方式非正式地演化，回应用户需求，考虑实现的细节。Lisp的非正式演化已经持续了很多年，并且Lisp社区的用户从来都抵制一个“官方”的语言定义（译者注：Scheme大概算反例了）。这种演化加上初始概念的灵活优美，使得Lisp，这个当今广泛使用的编程语言中第二古老的语言（只有Fortran更老），仍然可以适应涵盖大多数现代程序设计理念。因此，Lisp现在有一个大家族的方言*(dialects)*。这些不同方言虽然共享着大多数初始特性，但是也可能与彼此有很多地方有巨大的差异。这本书（译者注：即SICP）使用的方言叫做Scheme。

由于它的实验性以及对符号处理的强调，Lisp一开始在数据处理方面非常低效，至少与Fortran相比是如此。然而多年之间，产生了能够把Lisp代码转换为机器码的Lisp编译器，使Lisp程序可以较快地进行数据运算。并且在一些特殊的应用领域，Lisp发挥了巨大的作用。尽管Lisp尚未摆脱慢得无可救药的古老名声，但是它可以在很多效率不是第一要素的地方被使用。举一些例子，Lisp已经成为一种系统脚本语言的选择，或者是编辑器拓展语言（译者注：GNU Emacs），又或者是计算机辅助设计系统的语言。

既然Lisp不是一种主流语言，我们为什么还是要选择它作为我们讨论编程的平台呢？因为这种语言拥有一些与众不同的特性，是它成为学习重要的编程结构与数据结构并且阐明支持这两者的语言特性的绝佳媒介。这些特性中至关重要的一点是，Lisp描述过程的方式，也即procedure，可以用Lisp的数据结构来表示并且操作（译者注：在中文Lisp教材译本中常被称为“同像性”）。这个特性的意义在于一些强大的程序设计理念需要这种能力来消除被动数据与主动进程之间的传统界限。我们即将发现，Lisp的这种能力将使它成为世界上探索这些技巧最方便的语言之一。不仅如此，对于一些需要把其他程序作为代码来操作的程序，Lisp也借助这种把过程表示为代码的能力成为了一种绝佳的语言选择。

而最最最重要的是，用Lisp写程序，真的非常有趣。