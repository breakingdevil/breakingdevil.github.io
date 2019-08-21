---
layout:     post
title:      fake FILE struct
subtitle:   fake FILE
date:       2019-8-21
author:     breakingdevil
header-img: 
catalog: true
tags:
    - pwn
    - linux
    
---

# FILE struct
linux中标准IO库中使用FILE 结构体来描述文件的结构。此结构体会在调用open时创建。
```
struct _IO_FILE {
  int _flags;       /* High-order word is _IO_MAGIC; rest is flags. */
#define _IO_file_flags _flags

  /* The following pointers correspond to the C++ streambuf protocol. */
  /* Note:  Tk uses the _IO_read_ptr and _IO_read_end fields directly. */
  char* _IO_read_ptr;   /* Current read pointer */
  char* _IO_read_end;   /* End of get area. */
  char* _IO_read_base;  /* Start of putback+get area. */
  char* _IO_write_base; /* Start of put area. */
  char* _IO_write_ptr;  /* Current put pointer. */
  char* _IO_write_end;  /* End of put area. */
  char* _IO_buf_base;   /* Start of reserve area. */
  char* _IO_buf_end;    /* End of reserve area. */
  /* The following fields are used to support backing up and undo. */
  char *_IO_save_base; /* Pointer to start of non-current get area. */
  char *_IO_backup_base;  /* Pointer to first valid character of backup area */
  char *_IO_save_end; /* Pointer to end of non-current get area. */

  struct _IO_marker *_markers;

  struct _IO_FILE *_chain;

  int _fileno;
#if 0
  int _blksize;
#else
  int _flags2;
#endif
  _IO_off_t _old_offset; /* This used to be _offset but it's too small.  */

#define __HAVE_COLUMN /* temporary */
  /* 1+column number of pbase(); 0 is unknown. */
  unsigned short _cur_column;
  signed char _vtable_offset;
  char _shortbuf[1];

  /*  char* _save_gptr;  char* _save_egptr; */

  _IO_lock_t *_lock;
#ifdef _IO_USE_OLD_IO_FILE
};
```

FILE结构体通过_chain链成一个链表，链表头部用全局变量_IO_list_all表示。
stdin、stdout、stderr三个文件位于libc.so的数据段。
```
_IO_2_1_stderr_
_IO_2_1_stdout_
_IO_2_1_stdin_
```
我们在fake结构体时应该保证如下几点要求:
* _chain 指向一个FILE结构体
* _lock必须指向一个为NULL的空间
* _vtable_offset = 0
* _fileno >= 3
* _IO_jump_t 前两个为0x0
![](https://github.com/breakingdevil/breakingdevil.github.io/raw/master/img/2019-08-21/1.PNG)


![](https://github.com/breakingdevil/breakingdevil.github.io/raw/master/img/2019-08-21/2.PNG)

