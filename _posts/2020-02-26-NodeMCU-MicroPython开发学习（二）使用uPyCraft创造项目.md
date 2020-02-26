---
tags: ["ESP8266","IoT","Python","Chips"]
---

# 简述

作为一个基本上只用过C作为嵌入式开发语言的~~垃圾~~选手，虽然对于常见外设的功能和定义有基本的印象，但是对IDE的操作流程还是不熟悉的，而且Python始终都在《21天快速掌握Python》的水平，需要学习一个。

这部分其实有很多很棒的教学资料，有人在arduino.cn整理了完备的microPython开发资料，虽然是ESP32的，但仍然很有借鉴意义，需要自取：[点击跳转](https://mc.dfrobot.com.cn/thread-271930-1-1.html)，也可以按照我写的内容一步一步走，也许我会踩坑，但是这个过程何不是一种学习呢，我一定会讲明白的。其实我并不清楚，我的博客应该面对自己还是其他人，姑且用一种暧昧的语气，毕竟在我心里，如果自己的文档可以帮得上别人，这也算是一件很酷的事。

开发过程也可以参考：

MicroPython的[官方WiKi（英文版）](https://docs.micropython.org/en/latest/)；

官方文档的[星瞳科技翻译版](https://docs.singtown.com/micropython/zh/latest/openmvcam/#)；

> 文档夹杂了这家公司出品的OpenMV教学，好奇使然去他们官网看了一眼，打的旗号是要做机器视觉里的arduino，好嘛...一颗OV7725+STM32H7的机器视觉开发板居然要500+...有一点接受不能。

官方文档的[TPYBoard翻译版](http://docs.tpyboard.com/zh/latest/tpyboard/tutorial/)；

> 文档夹杂了这家公司自己的开发板使用的库的说明

综上所述，如果有能力的话，阅读官方版本可能效率最高...

其实最近也产生了很多想法，记录下来了不少，可惜一直没有足够的能力实现，万事开头难，加油。

首先可能需要尽快尝试运行一个MQTT吧。

# 实战操作

## 亮灯实验

之前只是尝试在命令行运行了一下命令，试一下使用IDE运行完整的项目的代码，亮灯实验作为单片机学习的Hello World，但重点不在此，主要目的还是在于学习uPyCraft的使用。

网上有的人是外接的灯，其实ESP8266的模块上自带了一盏蓝色的LED灯，在D2口，拉低即亮，可以先尝试在命令行运行一下：

```python
from machine import Pin
LED = Pin(2,Pin.OUT) #Pin.OUT == 1
LED.on()	#拉高灯灭
LED.off()	#拉低灯亮
```

实际效果如下：

![](https://cdn.jsdelivr.net/gh/UlyssesXC/imgulss/PIC12131.jpg)