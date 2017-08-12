---
title: 使用Keras进行回归
date: 2017-04-03 22:16:29
tags:
- keras
- hello
categories:
- Machine Learning

---

之前，我写了一篇[Hello Keras](/2017/03/25/Hello-Keras/)，写了用Keras做的异或神经网络。毕竟抄的是别人的代码，没啥意思。Keras很好上手，于是我写了一个线性回归和一个非线性回归，作为入门。

**（划重点）**Notebook在[这里](https://nbviewer.jupyter.org/url/linyxus.xyz/notebooks/keras-regression.ipynb)。官方给的View挺好用...看来自己写一个可以延后了。

<!-- more -->

## 线性回归

一个简单的一元线性回归，说实话一元线性回归都可以求出闭式解，根本用不着拟合，但身为一个~~菜鸡~~，这些有的没的都无所谓了。

思路很简单：

- 生成数据
- 建立网络（然而只有一个神经元）
- 训练
- 数据可视化

### 实现

```python
from keras.models import Sequential
from keras.layers import Dense, Activation
from keras.callbacks import History
import numpy as np
import matplotlib.pyplot as plt
```

Import一些必要的库，机械工作。

```python
x_data = np.random.random((80, 1))
y_data = x_data * 3.14 + 0.001516 + np.random.random((80, 1)) * 0.3
```

生成了一波数据，画个图看了看：

![kerasreg1.png](http://linfile.xyz/data/vip_data/kerasreg1.png)

效果还是挺不错的0v0

```python
# Define
model = Sequential()
model.add(Dense(1, input_dim=1))
model.compile(optimizer='sgd', loss='mse')
# Train
history = History()
model.fit(x_data, y_data, epochs=1000, batch_size=32, verbose=0, callbacks=[history])
losses = history.history['loss']
print losses[len(losses)-1]
```

建立网络，并训练。最后输出loss（这里是方差）。我这儿结果是0.00802586562932。由于生成的数据值基本在3以下，所以看不出是不是很好。

```python
x = np.linspace(0, 1)
y = model.predict(x)
plt.plot(x_data, y_data, '.', label='Sample Data')
plt.plot(x, y, '-', label='Regression Line')
plt.legend()
```

数据可视化，效果不错。

![kerasreg2.png](http://linfile.xyz/data/vip_data/kerasreg2.png)

## 非线性回归

这里~~随便~~，啊不，慎重挑选了一个函数。
$$
f(x) = x^3 - 2 x^2 + x
$$
这里只贴关键代码了，完整的Notebook上全有。

```python
# Define and train the model
model = Sequential()
model.add(Dense(32, input_dim=1, activation='relu'))
model.add(Dense(1))
model.compile(optimizer='sgd', loss='mse')
history = History()
model.fit(x_data, y_data, epochs=8000, batch_size=128, verbose=0, callbacks=[history])
losses = history.history['loss']
print losses[len(losses)-1]
```

最终结果：

![kerasreg3.png](http://linfile.xyz/data/vip_data/kerasreg3.png)

其实看了看loss的变化，发现其实还可以接着减小，但是囷于时间就训练8000次了。



