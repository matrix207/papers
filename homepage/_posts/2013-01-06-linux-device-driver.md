---
layout: post
title: "linux device driver"
description: ""
category: linux 
tags: [driver]
---



####\[转载\]Linux驱动开发学习的一些必要步骤  


_1._ 学会写简单的makefile

_2._ 编一应用程序，可以用makefile跑起来

_3._ 学会写驱动的makefile

_4._ 写一简单char驱动，makefile编译通过，可以insmod， lsmod， rmmod.   
    在驱动的init函数里打印hello world， insmod后应该能够通过dmesg看到输出。  

_5._ 写一完整驱动， 加上read， write， ioctl， polling等各种函数的驱动实现。   
    在ioctl里完成从用户空间向内核空间传递结构体的实现。  

_6._ 写一block驱动， 加上read，write，ioctl，poll等各种函数实现。

_7._ 简单学习下内存管理， 这个是最难的，明白各种memory alloc的函数实现细节。  
    这是Linux开发的基本功。  

_8._ 学习锁机制的应用，这个不是最难的但是最容易犯错的，涉及到很多同步和并发的问题。

_9._ 看内核中实际应用的驱动代码。   
   你会发现最基本的你已经知道了，大的框架都是一样的，无非是read，write，ioctl等函数的实现，  
   但里面包含了很多很多细小的实现细节是之前不知道的。  
   这时候就要考虑到很多别的问题而不仅仅是基本功能的实现。  
   推荐您看2.6.20中integrated的一个驱动kvm，记得是在driver/lguest下，很好玩的，就是Linux下的虚拟机驱动，代码不长，但功能强大。
   有能力的可以自己写一操作系统按照要求做成磁盘镜像加载到虚拟机中，然后客户机可以有自己的4G虚拟地址空间。

_10_. 看完驱动欢迎您进入Linux kernel学习中来。

最简单的方法，跟着ldd（Linux devive driver）做一遍

来源:  
[http://linux.chinaitlab.com/driver/745132.html](http://linux.chinaitlab.com/driver/745132.html)
