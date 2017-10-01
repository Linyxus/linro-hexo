---
title: Clojure HelloWorld [猜数游戏]
date: 2017-10-01 18:21:13
tags:
- tutorial
- helloworld
- clojure
categories:
- Clojure

---

最近买了黑客与画家，看到书评上说看完这本书可能会忍不住去学Lisp。果然，Paul大佬在书里把Lisp吹得神乎其神(233333)，买完这本书的后一周我就又买了本Living Clojure。

## 关于Clojure

Clojure是Lisp的方言，然而它基于JVM。Clojure的创造者说：

> 你可以不喜欢Java，但你可以喜欢JVM。

emmm，明显是因为JVM平台lib丰富才苟了一波嘛，还要这样安慰自己。\_(:зゝ∠)\_

<!-- more -->

**我为什么会选择Clojure呢？**

说实话，只是因为它的官网比较漂亮（而已）。

【正经脸】Lisp的另外两门热门方言（不包括Paul大佬开发的Arc）：CommonLisp和Scheme基本处于玩具语言的地位，而Clojure由于有JVM加持再加上本身语言特性（FP，Immutable Data）在网络、并行领域还是占有一席之地的。既可以作为玩具又可以有一定实际意义，~~（再加上官网比较好看）~~自然成为了我的首选。

## 猜数游戏

玩惯了OOP，对于Immutable Data真是一百不习惯。有时候忍不住把atom当变量用。然鹅，以递归方式把数据保存在调用栈里才是正道0v0

随便写了一个猜数游戏作为Hello World吧。

```clojure
(def diffi
  {1 100, 2 1000, 3 5000})
(defn menu []
  (println "choose difficulty:")
  (println (reduce #(str %1 "\n" %2)
                   (map #(str (first %) ") 1~" (last %)) diffi)))
  (let [inp (Integer/parseInt (read-line))]
    (if ((-> diffi keys set) inp)
      (get diffi inp)
      (recur))))
(defn -main []
  (def ans (rand-int (menu)))
  (loop [rounds 0]
    (do
      (println (str "round #" rounds))
      (println "guess:")
      (let [inp (Integer/parseInt (read-line))]
        (cond
          (= inp ans) (println "you win!")
          (> inp ans) (do (println "too big!") (recur (inc rounds)))
          (< inp ans) (do (println "too small!") (recur (inc rounds))))))))
```

