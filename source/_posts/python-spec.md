---
title: 使用Python创建音频频谱图
date: 2017-08-19 14:38:13
tags:
- python
- audio
- spectrogram
categories:
- Python
---

CNN不仅可以应用于图像识别，其实也可以应用于音频识别。处理的方法很简单，只要生成音频的频谱图，就可以使用CNN进行识别分类。

<!-- more -->

## 环境

-   Python
-   matplotlib
-   Scipy

## 转换格式

为了便于使用Scipy读取，我们需要把mp3格式文件转换为wav格式。方法很多，这里使用mpg123进行转换。

使用方法：

```shell
mpg123 -w output.wav input.mp3
```

## 读取音频

```python
from scipy.io import wavfile
sr, d = wavfile.read('1.wav')
channels = [
	d[:, 0],
    d[:, 1]
]
```

`wavfile.read`会返回两个值，`sr`为sampling rate，`d`则为读取到的音频信息。

一般来说，wav文件中可能包含多个声道，但我们只能对一个声道进行处理。处理哪个效果是差不多的。

## 生成频谱图

```python
import numpy as np
import matplotlib.pyplot as plt
import pylab
s, f, t, im = pylab.specgram(channels[1], Fs=sr)
im.figure
```

`specgram`会生成输入音频的频谱图，传入的第二个参数`Fs`为采样频率。

生成频谱图的过程是这样的：对输入音频做DTFT（短时傅里叶变换）。输出图像横轴为时间，纵轴为频率。根据各频率强弱对点着色，从而生成最终图像。

{% asset_img 0.jpg %}

但是，生成的图像中有坐标轴，会对CNN产生干扰，还需要去除`figure`中的坐标轴再保存。

```python
im.figure.gca().set_axis_off()
im.figure.savefig('pict.png', bbox_inches='tight', pad_inches = 0)
im.figure
```

{% asset_img 1.png %}

至此，我们完成了对一段音频频谱图的生成。

##  成果

最后，我们可以封装入一个函数：

```python
def spectrogram(path):
    sr, d = wavfile.read(path)
    channels = [
        d[:, 0],
        d[:, 1]
    ]
    s, f, t, im = pylab.specgram(channels[0], Fs=sr)
    im.figure.gca().set_axis_off()
    return im.figure
```

函数返回一个`figure`，下面可以对它进行显示、保存等等。

