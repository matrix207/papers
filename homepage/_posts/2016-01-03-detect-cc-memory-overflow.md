---
layout: post
title: "Detect c/c++ memory overflow"
description: ""
category: program 
tags: [c++, memory overflow]
---

#### Understand the process address space
* 0 to 0x08048000, keep for OS, not used.
* 0x08048000 to 0x40000000
  - read only of code and data
  - read and write of global variables
  - heap (malloc and free)
* 0x40000000
  - mmap allocate memory start from here, grow up
* 0xC0000000
  - the stack allocate start from here, grow down
* 0xC0000000 to 0xFFFFFFFF, use for kernel space

#### Stack and heap
For multiple threads program, OS will allocate a stack for every threads.
* Stack
  - ESP register point to the top of stack
  - EBP register point to the function action record
  - Parameters be pushed to stack from right to left
  - Return big data structure
  - Stack [from high to low]
    + Parameter N
    + Parameter ...
    + Parameter 2
    + Parameter 1
    + EIP (Next instruction address)
    + EBP
    + local variable 1
    + local variable 2
    + local variable ...
* Heap
  - malloc and mmap
    + `malloc` for small block memory, address bellow 0x40000000, and extend by brk/sbrk
    + When big block memory using `mmap`, address upper 0x40000000.
  - malloc: put the memory size before the real memory space to user

#### Failures
* stack overflow
  - failures
    + changed global variable
    + task not work, function not work
    + some local variable be changed
  - reason
    + thread stack too small
    + defined too large local variable
    + function called too deep
* global or heap overflow
  - failures
    + global variable be changed
    + memory leak
    + thread-A delete object but thread-B modify object again

#### Detect tools
* `mcheck`
  - disadvantage: only work on c memory manager, not work for new/delete of c++
* `mprotect`, set protection on a region of memory. For more see `man mprotect`
* `electric-fence`
  - <http://packages.debian.org/sid/electric-fence>
  - <http://xinsuiyuer.github.io/blog/2013/10/13/use-electric-fence-to-detect-heap-overruns-and-underruns/>
  - disadvantage: like mcheck: only work on c memory manager, not work for
    new/delete of c++
* `backtrac`
* `libsigsegv`

#### Reference
* [C++编译器越界检查机制](http://m.blog.csdn.net/blog/pecywang/22799705)
* [定位多线程内存越界问题实践总结](http://www.cnblogs.com/djinmusic/archive/2013/02/04/2891753.html)
* [多线程内存问题分析之mprotect方法](http://www.yebangyu.org/blog/2016/02/01/detectmemoryghostinmultithread/)
* [一次内存越界的 bug](http://blog.codingnow.com/2014/01/out_of_range_bug.html)
* [Linux C语言 内存越界问题总结](http://m.blog.csdn.net/blog/ring0lyy/8279286)
