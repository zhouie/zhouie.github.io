---
title: 【Python学习实践】 speech智能语音模块
categories:
  - Python学习实践
tags:
  - Python
  - speech
abbrlink: 201807271
date: 2018-07-27 20:25:11
---

> 🤷‍️最近的生活、学习节奏很是容易被打断，终于，在今天，既实习结束之后，夏令营也结束了。
> 前几天，一个人在复习地很累的时候，又重新将Python捡了起来，看了挺多的知识点。 真是太有意(wu)思(liao)了！

### 环境准备

1️⃣  `python2.*` 或 `python3.*`

2️⃣  安装`pywin32`扩展库

3️⃣  安装`speech`模块

#### 安装Python 2/3

* 安装Python2还是Python3的选择上，我个人是推荐两个都装上，在某些地方用的时候稍微多个切换环境变量的步骤而已。当然了，我觉得就目前Python3已经推出的时长来看，初学者的话，还是建议先选择安装Python2熟悉Python的一些语法结构定义，而且使用上应该较舒适一些吧。毕竟Python2推出的时间更久一些，遇到一些问题，可供搜索的解决方案也丰富些；同时啊，目前仍让存有部分模块功能无法适应Python3或是Python3.*的较高版本。
* 具体安装的的过程就不详讲了，不管哪个版本都几乎大同小异，很容易上手也是Python的特性之一
* 安装完Python环境之后啊，毕竟也是一门编程语言，选择一个合适的IDE是必不可少的，这里的话，我推荐阅读一下[Python IDE](http://www.runoob.com/python/python-ide.html)，更多 Python IDE 请参阅：[http://wiki.python.org/moin/PythonEditors](http://wiki.python.org/moin/PythonEditors)

#### PyCharm IDE

在众多IDE中，最受大家追捧的一款便是PyCharm了，作为 **JetBrains**全家桶的一份子，这款软件具备完善功能，如：调试、语法高亮、项目管理、代码跳转、智能提示、自动完成，单元测试甚至版本控制等。另外，它还提供了一些很好的功能用于 **Django 开发**，同时支持 **Google App Engine**，更酷的是，PyCharm 支持 **IronPython**。

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/speech/1.png)

官方下载地址：[http://www.jetbrains.com/pycharm/download/](http://www.jetbrains.com/pycharm/download/)

IntelliJ IDEA 注册码:[http://idea.lanyus.com/](http://idea.lanyus.com/)以及提供的[破解帮助文档](http://idea.lanyus.com/help/help.html)

IDEA系列主题下载地址：[http://www.riaway.com/theme.php](http://www.riaway.com/theme.php)，这些主题支持的IDE包括：InteliJ IDEA, PhpStorm, PyCharm, RubyMine, WebStorm and AppCode。

注：怎么安装下载的主题

1. 从主菜单打开你的编辑器选择` File->Import Setting` ，选择你下载的jar文件;
2. 等待重启之后进行配置：打开` File->Settings->Editor->Colors and fonts`，然后选择你安装的主题即可完成。

#### 安装pywin32扩展库

pywin32即 `Python for Windows Extensions`，提供了Pyhton访问和调用Windows底层功能函数的接口，pywin32包括了`win32api`、`win32com`、`win32gui`、`win32process`等模块

下载地址：[https://sourceforge.net/projects/pywin32/files/pywin32/Build%20221/](https://sourceforge.net/projects/pywin32/files/pywin32/Build%20221/)

这个要 **根据Python版本（2.\*/3.\*）和CPU位数（32位/64位）下载相应版本并安装**。

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/speech/2.png)

比如，像我这种情况，就需要安装 `pywin32-221.win-amd64-py3.7.exe`

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/speech/3.png)

#### 安装speech模块

使用`pip install speech`命令即可安装，但对于安装了Python3的用户来说，还需要针对Python3的一些新特性对安装后的配置文件做些修改

> 至于这里的`pip`，它是Python包管理工具，该工具提供了对Python包的查找、下载、安装、卸载的功能；
> 同时，`Python 2.7.9 +` 和 `Python 3.4+` 以上版本都自带pip工具，只要安装的时候没取消勾选那个选项，一般都不用再特别安装了的，可以通过命令`pip --version`来判断是否已安装；
> pip官网下载地址:[https://pypi.org/project/pip/](https://pypi.org/project/pip/)，注意，用哪个版本的 Python 运行安装脚本，pip 就被关联到哪个版本；
> 值得注意的是，部分 Linux 发行版可直接用包管理器安装 pip，如 Debian 和 Ubuntu：`sudo apt-get install python-pip`。

#### pip常用相关指令

| 功能    | 指令 |
| :-:   | :-: |
| 显示版本和路径 | pip --version |
| 获取帮助 | pip --help |
| 升级 pip | pip install -U pip |
| 安装最新版本包 | pip install SomePackage |
| 安装指定版本 | pip install SomePackage==1.0.4 |
| 最小版本 | pip install 'SomePackage>=1.0.4' |
| 升级包 | pip install --upgrade SomePackage |
| 升级至指定的包 | 使用== 、 >= |
| 卸载包 | pip uninstall SomePackage |
| 搜索包 | pip search SomePackage |
| 显示安装包信息 | pip show |
| 查看指定包的详细信息 | pip show -f SomePackage |
| 列出已安装的包 | pip list |
| 查看可升级的包 | pip list -o |

注：如果上面那个 **升级pip命令**出现问题 ，可以使用以下命令：`sudo easy_install --upgrade pip`

#### Python3.*环境下正常使用speech的解决方法

[speech's Project description](https://pypi.org/project/speech/0.5.2/#description)

安装完speech模块后，需要去修改 **speech.py**文件，该文件路径在`..\Python37\Lib\site-packages`下

1. line59 修改`import thread`，改成`import threading`；
2. line157 修改`print prompt`，改成`print(prompt)`；
3. 对最后的函数`_ensure_event_thread`修改如下：

```python
class T(threading.Thread):
    def __init__(self):
        threading.Thread.__init__(self)
    def run(self):
            pass
        
def _ensure_event_thread():
    """
    Make sure the eventthread is running, which checks the handlerqueue
    for new eventhandlers to create, and runs the message pump.
    """
    global _eventthread
    if not _eventthread:
        def loop():
            while _eventthread:
                pythoncom.PumpWaitingMessages()
                if _handlerqueue:
                    (context,listener,callback) = _handlerqueue.pop()
                    # Just creating a _ListenerCallback object makes events
                    # fire till listener loses reference to its grammar object
                    _ListenerCallback(context, listener, callback)
                time.sleep(.5)
        _eventthread = T()
        _eventthread.start()
```

### 智能语音自动读词脚本

前几天，朋友分享给我一个关于计算机专业相关英语词汇的文件，接收之后一直放在电脑桌面上，也不知道有没有用。今天下午的时候，因为最近的夏令营活动也一直有和AI相关研究领域团队交流，所以突然就想到可不可以将这些英语单词用已有的Python语音扩展包去实现一种类似于"课堂听写"的自动(谈不上智能)模式。

具体的源程序下面也可以看到，整个的程序结构很简陋，也没花多少时间，相信理解起来也并不难。

值得说明的有这么几点：

1. 提供的样例单词都是计算机专业相关的，如果你想换成别的单词，只需要修改`test.csv`文件即可。其中，`test.csv`文件中共有两列数据，分别代表英语词汇与相应中文释义，要想实现中英互译的功能，只要去将这列的顺序去对调一下即可。
2. 和"课堂听写"模式相像，每次会相隔`Interval_Time`时间按行语音输出词汇两次（听写的时候，一般老师会间隔着地读2~3次），之后的话，每听写一小组词汇（LOOP_NUM个），就会将它们一起展示出来（中英对照），也以便看看自己的"正确率"有多高。
3. 在选择哪个智能语音模块上，其实也有做了很多了解和实验
    - 最开始是打算用[pyttsx](https://github.com/RapidWareTech/pyttsx)（Python3好像要用[pyttsx3](https://github.com/nateshmbhat/pyttsx3)），期间碰到了不少的问题，尤其对于Python3来说，只能多去搜搜看了，[pyttsx的中文语音识别问题及探究之路](http://www.cnblogs.com/leenid/p/6875031.html)、[pyttsx3 - Text-to-speech x-platform](https://pyttsx3.readthedocs.io/en/latest/)、[py库：文本转为语音（pywin32、pyttsx](https://www.cnblogs.com/qq21270/p/7899622.html)
    - 之后也接触到了[pydub](http://pydub.com/)，pydub需要依赖[libav](https://libav.org/)或者[ffmpeg](https://zhouie.cn/posts/201807241)，推荐阅读：[五十音听写：Python 音频处理库 pydub](https://www.jianshu.com/p/a9291fa603f6)、[python音频处理库：pydub](http://appleu0.sinaapp.com/?p=588)。
4. 既然谈到这儿，就多聊一些关于音频方面的内容，比如，如何使用Python播放`mp3`、`wav`、`ogg`格式的音频文件
    - 调用系统默认播放器播放
    
    ```python
    import time
    import os
    file = r'F:/Test/musicT/Hello.mp3'
    os.system(file)
    time.sleep(10)
    ```
    - pygame 播放，但存在语速失真的不足，pygame提供了两个加载音乐文件的方法
        + pygame.mixer.Sound，主要加载ogg和wav音频文件。
        + pygame.mixer.music，主要加载mp3音频文件。

    ```python
    import time
    import pygame
    file = r'F:/Test/musicT/Hello.mp3'
    pygame.mixer.init()
    track = pygame.mixer.music.load(file)
    pygame.mixer.music.play()
    time.sleep(10)
    pygame.mixer.music.stop()
    ```
    - mp3play播放，语速正常，但貌似目前只能用于python2.\*，不支持python3.\*
    
    ```python
    import time
    import mp3play

    def playmusic(path):
        clip = mp3play.load(path)
        clip.play()
        time.sleep(10)
        clip.stop()

    file = r'F:/Test/musicT/Hello.mp3'
    playmusic(file)
    ```
5. 在编程过程中，有遇到了下面这种问题，针对这种情形的解决方案便是去任务管理下找到并结束智能语音进程。

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/speech/4.png)

### 源程序

当然，也可以[去GitHub上Download Latest Version](https://github.com/zhouie/vir_speaker)，嗯emmm..，提前感谢您的`star` ⭐

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Time    : 2018/7/27 18:01
# @Author  : zhouie
# @File    : main.py
# @Software: PyCharm

import speech
import time
import csv

data = []  # 暂存 *.csv 文件每行数据
Interval_Time = 4  # 两次读词的间隔时间
LOOP_NUM = 8  # 循环基数
Think_Time = 15  # 回顾等待时间

csv_file = open('./res/test.csv', encoding='utf-8')
csv_reader_lines = csv.reader(csv_file)
# print(csv_reader_lines)

num = 0
for one_line in csv_reader_lines:
    data.append(one_line)
    num = num + 1

speech.say("计算机专业相关的英语单词 中英互译 测试小程序，demo版")
speech.say("This is a small routine (compiled by Python) for exercise about English phrases in the field of computer")

i = 0
while i < num:
    # print(i + 1, data[i][0])
    # speech.say(i + 1)
    # speech.say(data[i][0])
    # time.sleep(Interval_Time)
    # speech.say(data[i][0])
    if 0 == (i + 1) % LOOP_NUM:
        speech.say("来回顾一下 以上所学的几个词汇吧")
        speech.say("Just follow me , look back on the words you have learned...")
        print("!--#######--*--#######--!")
        print("第", int(i / LOOP_NUM) + 1, "组词汇：")
        for j in range(i - (LOOP_NUM - 1), i + 1):
            print(data[j][0], data[j][1])
            speech.say(data[j][0])
            speech.say(data[j][1])
        print("!--#######--*--#######--!")
        speech.say("你的正确率如何呢？")
        speech.say("So , What about your correct rate?")
        time.sleep(Think_Time)
    i = i + 1

speech.say("Congratulation!")

``` 

### 效果展示

<iframe frameborder="0" width="100%" height="500" src="https://v.qq.com/iframe/player.html?vid=o0737zviriw&tiny=0&auto=0" allowfullscreen>
