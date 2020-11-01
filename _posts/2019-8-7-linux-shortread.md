---
layout:     post
title:      linux short read
subtitle:   short read
date:       2019-8-7
author:     lycium
header-img: 
catalog: true
tags:
    - linux
    
---

# 实验记录
今天在调试程序的时候遇见了玄学的bug, linux short read, 直译过来就是短读. 一般来说我们在使用read函数时，read函数的返回值为读取到的字节数，read是允许返回的字节数少于你传参字节数，所以这样就会产生短读。一般而言我们的计算机的磁盘文件系统使用的是不可中断的read操作，所以我们很少在本地的文件系统中碰见短读操作。比如你在使用NFS(network file system)时就有可能出现短读，因为NFS使用的read是可以被中断的read，于是出现了部分的返回字节，这也就是为什么有时会出现玄学bug。
