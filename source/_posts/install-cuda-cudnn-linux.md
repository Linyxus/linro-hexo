---
title: Ubuntu16.10为Tensorflow安装CUDA8.0与cuDNN
date: 2017-08-02 16:41:42
tags:
- cuda
- cudnn
- tutorial
categories:
- Machine Learning

---

如果有一块好的GPU，那么对模型训练的加速是非常显著的，比如我i3训练一个上百万参数的神经网络，数据集50000条，一个epoch需要整整400秒，但是用GTX750Ti加速，一个epoch只要30秒。本来要训练一整天的模型，挂在那里两个小时就训练完毕了。

为了使用GPU加速神经网络的计算，CUDA是一个不错的选择。当然，别忘了安装tensorflow-gpu版本。

<!-- more -->

**Tips** 由于CUDA安装包较大，当你看到这篇教程的时候就可以开始[下载](https://developer.nvidia.com/cuda-downloads)了。这篇教程针对Runfile安装方法撰写，deb其实可以更加简单一些。同样可以开始下载的还有[cuDNN](https://developer.nvidia.com/rdp/cudnn-download)，选择cuDNN v5.1 for CUDA 8.0。

## 安装前的确认

这是官网[Guide](http://developer2.download.nvidia.com/compute/cuda/8.0/secure/Prod2/docs/sidebar/CUDA_Installation_Guide_Linux.pdf)上的步骤，但是我觉得必要性不大。但还是在这儿说一下。

- GPU支持Cuda [List](http://developer.nvidia.com/cuda-gpus)

- 保证Linux版本受支持。（其实16.10是不受支持的，但是也可以成功安装）

- 安装了gcc

  这个可以提一下，网上很多地方都说gcc必须降级，并且runfile其实也会检测gcc的版本，太高会给出error，但我们安装cuda的目的是给Tensorflow用，所以gcc的版本其实并不重要，忽略即可。

- 保证Development packages与内核版本对应。（如果没自己改过内核，一般都是对应的）

- 下载Cuda

- 处理安装冲突

很多教程里都说要先禁用nouveau否则会对安装造成影响，其实是不用的。（至少在我这儿的安装环境是这样）

至于Stack Overflow里那个很繁琐的安装过程...emmm...我也不懂。

## 安装CUDA

```sh
sudo sh {cuda}.sh --override
```

> 如果提示unsupport version也不用管，Ubuntu16.10是可以成功安装的
>
> --override`是为了避免产生gcc版本错误中断安装

基本没什么好说的，一路yes。

重启一下电脑，试试有没有安装成功：

```sh
nvidia-smi
```

如果显示gpu信息就是成功了，sample code就别编译了，gcc版本不对会报错。

## 安装cuDNN

请注意一定要选择v5.1版本，因为Tensorflow使用的是cuDNNv5.1的接口，v6.0版本是否可以是未知的。

```sh
tar xvzf cudnn-8.0-linux-x64-v5.1.tgz
sudo cp cuda/include/cudnn.h /usr/local/cuda/include
sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64
sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
```

## 添加环境变量

在.bashrc,.zshrc的最后，添加：

```sh
export PATH=/usr/local/cuda-8.0/bin${PATH:+:${PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
export CUDA_HOME=/usr/local/cuda
```

这些环境变量是必要的。

最后，可以重启一下系统。