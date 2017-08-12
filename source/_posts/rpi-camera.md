---
title: 为树莓派装上摄像头
date: 2017-08-11 19:30:54
tags:
- RPi
- tutorial
- helloworld
categories:
- Raspberry Pi

---

前几天刚刚买了一个Raspberry Pi 3，顺便也买了一些配件来玩。RPi是自带摄像头接口的，买一个摄像头模块就可以直接装上去。正好买的模块套餐里有一个摄像头\_(:зゝ∠)\_

<!-- more -->

## ArchLinux ARM中摄像头模块的配置

如果用的是raspbian，官网上有详细教程。

Arch上如果不配置，开启摄像头会报错...Google了一下找到了解决方案。

### 修改PATH

需要把`/opt/vc/bin`添加到PATH中：

```sh
export PATH=$PATH:/opt/vc/bin
```

### 修改config.txt

修改`/boot/config.txt`：

```
gpu_mem=128
start_file=start_x.elf
fixup_file=fixup_x.dat
```

## 安装摄像头至树莓派

PCB线的安装其实还是很简单的。首先找到Camera的PCB口，（树莓派有两个PCB口，还有一个叫DISPLAY），然后把上面黑色的卡口掰开，将摄像头的带状PCB口插入，注意方向，有铁片的放一起，并且检查是否对齐，最后按下卡扣，扣紧。稍微用力拉一拉，拉不出来即为安装成功了。

![o_1bna5ftd7gn0su81ll22l3ktia.jpg](http://ok30v00pz.bkt.clouddn.com/o_1bna5ftd7gn0su81ll22l3ktia.jpg)

## 安装PiCamera

这是一个强大的Camera模块库，提供较高层的API，不用麻麻烦烦地和底层打交道啦。

如果使用的是raspbian，那picamera应该是预装的。而我用的是Archlinux所以要安装一下，用pip就行了。

```sh
sudo pip install picamera
```

## 使用picamera

这里仅仅列举几个比较简单的用法，其实picamera还支持图像处理之类的，加个特效什么的不是问题，但这里先不涉及（因为我还没用过2333）。

### 拍摄照片

```python
from picamera import PiCamera
import time
camera = PiCamera()
camera.start_preview()
print("Preview started! Waiting for 5 sec...") # 主要目的是等待摄像头对焦
time.sleep(5)
for x in range(5): # 连拍5张照片
    print("Taking photo #%d" % x)
    camera.capture('/home/alarm/%d.jpg' % x)
    time.sleep(1)
camera.stop_preview()
```

简单总结一下，首先获取一个instance，保存为camera。然后开启摄像头，`start_preview()`。下面等待一会儿，让摄像头对焦。拍摄照片使用的是`capture`，参数是保存路径。最后，关闭摄像头，`stop_preview()`。

### 录制视频

```python
from picamera import PiCamera
import time
camera = PiCamera()
camera.start_preview()
print("Preview started. Recording started.")
camera.start_recording("/home/alarm/video.h264")
print("Recording...")
time.sleep(10)
print("Finished!")
camera.stop_recording()
camera.stop_preview()
```

与拍摄照片唯一的不同就是使用了`start_recording("xxx")`和`stop_recording()`，参数同样是文件保存位置。这里共录制了10s。保存的文件格式为h264，picamera模块还支持mjpeg，但是不建议使用后者，因为文件大小相去甚远...

emmm，要写的就这么多。