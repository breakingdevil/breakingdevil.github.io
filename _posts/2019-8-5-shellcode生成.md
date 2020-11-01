---
layout:     post
title:      shellcode生成
subtitle:   shellcode
date:       2019-8-2
author:     lycium
header-img: 
catalog: true
tags:
    - pwn
    
---

# shellcode的生成
  我们在练习pwn的时候如果有可写可执行的段即可直接在shellcode-storm上直接搜索到，如果有条件限制可以使用msf的msfvenom生成工具生成，zhiyishellcode的使用条件。最次的情况就是自行叠rop链然后再进行相关的操作。
