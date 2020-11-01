---
layout:     post
title:      linux main 函数执行的宏观过程
subtitle:   linux main process
date:       2019-8-9
author:     lycium
header-img: 
catalog: true
tags:
    - linux
    - reverse
---

# 程序main的出生到死亡
linux ELF程序的执行当然是从_start函数开始执行的啦，但是我们一般编写main函数的时候却是直接在main函数中进行我们的编程工作。下面让我们来分析一下大体的执行过程。
## Glibc 的入口实现
```
void _start()
{
　　%ebp = 0;
　　int argc = pop from stack
　　char ** argv = top of stack;
　　__libc_start_main(main, argc, argv, __libc_csu_init, __linc_csu_fini,
　　edx, top of stack);
}
```
一般而言，这是一个linux程序的执行入口的样子。__libc_start_main的函数原型定义我们可以找到相关的文档。
```
int __libc_start_main(int *(main) (int, char * *, char * *), int argc, char * * ubp_av, void (*init) (void), void (*fini) (void), void (*rtld_fini) (void), void (* stack_end));
```
那么我们在反汇编的时候根据amd64的调用约定参数分别为rdi,rsi,rdx,rcx,r8,r9,栈顶第一个参数。
main一般就是我们写的main函数，argc是main的参数个数，ubp_av就是我们的argv,init函数主要进行main的前初始化工作，fini执行main的收尾工作，rtld_fini执行main的动态链接相关的收尾工作，最后一个则为栈顶指针。
打破砂锅问到底，那么__libc_start_main函数里面进行了哪些操作呢？
## __libc_start_main 内部工作原理
首先程序会检查是不是多个库.如果这个后面的代码就会根据程序是否开启PIE而执行PIE的操作。后面会判断是否开启AUX_VECTOR,如果开启了，将会将本程序的aux vector链到链表末尾。链接器可以让__ehdr_start指向我们的ELF头，我们可以在没有任何来自auxv的信息情况下初始化_dl_phdr和_dl_phnum。
接下来会初始化lib安全相关操作。接着初始化参数。接下来的三个初始化会根据CPU架构来进行初始化，分别为IREL，TLS，和APPLY_IREL。接下来初始化的就为stack guard即canary。后面初始化的为poniter_check_guard。如果我们的ELF为动态链接库那么接下来就会注册rtld_fini函数。我们接下来被初始化的就为我们main函数所获得到的参数，argc，argv，env。接下来init区节内的的函数将被执行，如果是动态链接库则会审计新的入口点。接下来就会根据HAVE_CLEANUP_JMP_BUF完成后续的操作最后call main函数。
