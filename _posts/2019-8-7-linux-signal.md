---
layout:     post
title:      linux signal
subtitle:   linux signal
date:       2019-8-2
author:     枸杞蒲蒻
header-img: 
catalog: true
tags:
    - linux
    
---

# linux signal machenism
linux 支持posix reliable signals 和 posix real-time signals
## linux signal 规定
每个signal都有自己的规定，规定他们在信号被送达时候进程该如何反应。有一些signal 有他们默认的行为，Term 终止当前进程，Ign 忽略当前信号，Core 终止进程生成dump文件，Stop 停止当前进程， Cont 如果当前程序停止则将它继续运行。我们可以用sigaction 和 signal 来改变一些signal的规则，比如使用系统默认的行为，忽略该信号，或者使用我们自己定义的handler函数。一般而言signal调用会在普通的进程栈上，我们可以用signalstack来改变handler的stack。
signal属于进程属性，即使在多线程的程序中所用的线程使用同一套signal处理方案。
如果我们使用fork创建子进程，则子进程会继承父进程的所有signal，如果我们是execve，那么原先我们handler过的信号则会被系统恢复为初始handler，但是被设置为ignore的信号仍然被忽略。
更多内容请读文档，写的很详细，不想再翻译一遍了。:(
