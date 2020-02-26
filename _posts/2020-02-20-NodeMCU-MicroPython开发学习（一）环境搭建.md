---
tags: ["ESP8266","IoT","Python","Chips"]
---

# 简述

疫情闲来无事，搭好博客也不知道有什么可以写的，手上别说树莓派，除了3年前实验室大一玩完的Node MCU ESP8266以外，暂时什么都没有，想了想，折腾一下这块板子也还行，结果查完才发现这块板子的强大，不止是Arduino，Lua、MicroPython居然都支持，那感情好啊，拿它随便玩玩，Python写个比特币行情监控机器人，还不是美滋滋~~（现在还不太懂）~~。

# 搭建环境

要想NodeMCU支持MircoPython开发，首先需要做的事：

- 给MCU刷写MicroPython专门的固件；
- 安装PC端开发环境（Windows10）；
- 自己还想折腾的事：在Ubuntu18.04搭建开发环境。

那就按步骤边做边写吧。

## 下载+烧录MircoPython固件

### 什么是MicroPython

MicroPython这东西还是挺牛逼的，Sipeed推出的板子也在宣传自己支持MicroPython开发，可见支持这个东西还是很有意义的。

>MicroPython由C编写，是 [Python 3 ](http://www.python.org/)语言 的精简高效实现 ，包括**Python标准库的一小部分**，经过优化可在微控制器和受限环境中运行。
>
>MicroPython包含了诸如交互式提示，任意精度整数，关闭，列表解析，生成器，异常处理等高级功能。 足够精简，适合运行在只有**256k的代码空间和16k的RAM的芯片**上。
>
>除了ESP8266以外，MicroPython现已支持ESP32、STM32F4的几款芯片、ST官方的ST官方的Nucleo系列芯片，德仪的CC3200~~（没细查）~~，甚至还有F103的[Github民间支持版本](https://github.com/mcauser/micropython/tree/stm32f103)

具体可见[MicroPython官网](http://www.micropython.org/)的相关内容。

### 烧录准备

我们需要几款工具：

- NodeMCU驱动支持：CP340或CP2102等等
- MicroPython固件：[点击下载](http://www.micropython.org/resources/firmware/esp8266-20191220-v1.12.bin)（时间久了就不一定是最新版了）
- Windows10环境的ESP8266烧录工具：[点击下载](https://www.espressif.com/zh-hans/support/download/other-tools)

### 烧写

打开在乐鑫官网下载的烧录工具，点击ESP8266，按下图填写：

![](https://cdn.jsdelivr.net/gh/UlyssesXC/imgulss/PIC20200220001608.png)

**建议在刷固件前，先ERASE一下，把旧的删掉**

点击START，开始刷写固件。

![](https://cdn.jsdelivr.net/gh/UlyssesXC/imgulss/PIC20200220002907.png)

烧录完成后，我们打开串口工具，我用的是正点出的XCOM，如果在RTS后接受到了打印信息，就算烧写成功了。

![](https://cdn.jsdelivr.net/gh/UlyssesXC/imgulss/PIC20200220005623.png)

### Hello World！

通过串口发送命令过去，就可以直接当作命令行运行！

试试看`help()`

![](https://cdn.jsdelivr.net/gh/UlyssesXC/imgulss/PIC20200220010641.png)

打印`Hello World`

![](https://cdn.jsdelivr.net/gh/UlyssesXC/imgulss/PIC20200220010908.png)

## 搭建IDE环境

网上有很多不同的图形化的IDE，基本上分成专有的和支持插件的编辑器两类，前者包括*uPyCraft*，后者则理所应当的有*Pycharm*、*VS Code*。

### uPyCraft IDE搭建

非常没难度 只要下载即可使用了，链接可见：[Introduction](https://dfrobot.gitbooks.io/upycraft/content/)。

打开界面如图所示：

![](https://cdn.jsdelivr.net/gh/UlyssesXC/imgulss//PIC20200226204105.png)

在 Tools 中按顺序选好芯片型号和端口，点击右侧的锁链标志连接即可。

成功连接后下方命令行窗口即可正常使用。

### ~~VS Code IDE搭建~~（遇到一点问题）

因为对*VS Code*的偏爱，开始时选择了先试试*VS Code*。

在插件库搜索`Pymakr`，点击安装。

![](https://cdn.jsdelivr.net/gh/UlyssesXC/imgulss/PIC20200220233850.png)

> 大致翻译一下：Pymakr支持用户和Pycom开发板通信，支持使用命令行REPL、运行单个文件，或同步完整的项目到芯片

安装完，编辑器的右下角可能会提示没有NodeJS环境，前往官网[下载安装](https://nodejs.org/en/)。

流程没什么好说的，安装完成后直接打开cmd，输入`node -v` 检查是否安装成功：

![](https://cdn.jsdelivr.net/gh/UlyssesXC/imgulss/PIC20200221001049.png)

重启Code，编辑器会自动弹出`pymakr.json` 配置文件，将内容替换为：

```
    {
    "address": "COM19",//根据自己的端口号改
    "username": "micro",
    "password": "python",
    "sync_folder": "/flash",
    "open_on_start": false,
    "sync_file_types": "py,txt,log,json,xml,html,js,css,mpy",
    "ctrl_c_on_connect": false,
    }
```

替换完点击保存，更新json，启动终端。

右下角有按钮对应的功能：

- **Pycom Console：**打开或关闭与板子的链接
- **Run：**运行当前文件
- **Upload：**上传工程文件到板子里
- **Download：**下载板子里的工程文件
- **All commands：**支持的命令操作

**出现问题**：按照网上的教程，这里本该连接成功了，但是实际操作的时候会发现调试台搜索不到设备。

![](https://cdn.jsdelivr.net/gh/UlyssesXC/imgulss/PIC20200221003344.png)

点击`All commands` 中可以发现，配置文件保存完没有及时更新。

结束

***后面开始尝试各个库和Python的对比，再学一下基本硬件的调用。***

