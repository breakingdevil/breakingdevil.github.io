---
layout:     post
title:      chunk-extend-and-overlap
subtitle:   chunk-extend-and-overlap
date:       2019-9-2
author:     lycium
header-img: 
catalog: true
tags:
    - linux
    - pwn
    
---


# chunk extend overlap条件
- 程序中需要存在基于堆的漏洞
- 漏洞需要可以控制chunk header的数据


# 攻击举例
- 对inuse的fastbin进行extend
- 对inuse的smallbin进行extend
- 对free的smallbin进行extend
- 通过extend后向overlap
- 通过篡改presize size实现向前overlap
