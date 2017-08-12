---
title: Hello Keras
date: 2017-03-25 19:58:10
tags:
- keras
- test
- hello
- python
categories:
- Machine Learning
---
## Hello Keras
用[Tensorflow](https://www.tensorflow.org/)写代码着实不是一件让人享受的事情，一个简单的面拟合可能就有数十行。

而[Keras](https://keras.io/)是一个高度模块化的深度学习库，用它写神经网络比Tensorflow舒服多啦。

<!-- more -->

例如训练一个异或神经网络，Tensorflow有百来行很正常，然而...

```Python
#Import Libraries
import numpy as np
from keras.models import Sequential()
from keras.layers.core import Activation, Dense
#Datas
training_data = np.array([[0,0],[0,1],[1,0],[1,1]], "float32")
target_data = np.array([[0],[1],[1],[0]], "float32")
#Set up model
model = Sequential()
model.add(Dense(32, input_dim=2, activation='relu'))
model.add(Dense(1, activation='sigmoid'))
model.compile(loss='mean_squared_error', optimizer='adam', metrics=['binary_accuracy'])
#Train
model.fit(training_data, target_data, epochs=1000, verbose=2)
#Test
print model.predict(training_data)
```

0v0Keras十几行代码轻松搞定。

见Notebook的[Demo](http://linyxus.xyz/notebooks/Hello+Keras.html)。

## 安装
```shell
sudo pip install keras
```

## 题外话
发现了一个小问题，JupyterNotebook导出的HTML即使没写几个字，也足足有几百Kb。因为里面内置了上万行的CSS...
现在Notebooks是目录浏览，只是权宜之计，以后会考虑搭一个平台，用类似于InstantClick的方法，当然也可能直接用InstantClick。
