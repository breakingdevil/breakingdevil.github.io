---
layout:     post
title:      heapoverflow-and-off-by-one
subtitle:   heap attack
date:       2019-8-30
author:     lycium
header-img: 
catalog: true
tags:
    - pwn
    - linux
    
---


# 堆攻击技巧:heap over flow 和 off by one
一般情况而言一下两个可以发生堆溢出的前提
* 程序向堆上写入数据
* 写入的数据大小没有被很好的控制

我们利用堆溢出的基本策略
* 覆盖下一个chunk的prevsize、size、chunk_content
* 利用堆的机制来进行数据的写入例如unlink

一下函数可能会发生溢出
* gets
* scanf
* vscanf
* sprintf
* strcpy 遇到\x00停止\x00会追加
* strcat 遇到\x00停止\x00会追加


ptmalloc分配出来的大小是对齐的，一般长度会是机器字长的两倍。
比如 32 位系统是 8 个字节，64 位系统是 16 个字节
注意用户区域的大小不等于 chunk_hear.size，chunk_hear.size = 用户区域大小 + 2*字长

# off by one
一般是程序逻辑问题导致str多溢出一个字节

off-by-one 利用思路
* 溢出字节为可控任意字节：通过修改块结构之间出现重叠，从而泄露其他块数据，
* 溢出的字节为NULL使得下一块的prev_in_use被清除，pre_size被启用，可以伪造，使得块重叠。此方法的关键在于 unlink 的时候没有检查按照 prev_size 找到的块的后一块（理论上是当前正在 unlink 的块）与当前正在 unlink 的块大小是否相等。

2.28前没有check
还未在实践中测试此攻击方法。
