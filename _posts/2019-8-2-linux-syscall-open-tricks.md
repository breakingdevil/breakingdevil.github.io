---
layout:     post
title:      linux syscall read 一点探究
subtitle:   syscall read
date:       2019-8-2
author:     枸杞蒲蒻
header-img: 
catalog: true
tags:
    - linux
    
---

# linux 系统是一个复杂又有趣的操作系统
今天了解了一下linux的syscall的魅力，我们为什么在编程中使用linux的syscall呢？这样的程序可以极大程度的减少对于glibc的依赖，编译的出来的程序小巧玲珑。更重要的一个特点是我们可以极大可能的利用系统机制去解决一些比较棘手的问题。
今天我学习了一下syscall read，和他相关的两个syscall为name_to_handle_at和open_by_handle_at。这两个syscall将openat分割，主要的函数功能我们可以在man里查询到，比如我们的函数open坏掉的时候，我们可以用这两个syscall代替open的功能。linux手册上有的内容我就不再赘述，这样的操作主要能让用户级程序能够映射文件名处理并且后续可以使用不同文件系统的句柄进行操作。这样能减少服务器的压力，并且可以产生持续性的文件操作。
