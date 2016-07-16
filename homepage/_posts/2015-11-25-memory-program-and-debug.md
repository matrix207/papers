---
layout: post
title: "memory program and debug"
description: ""
category: program
tags: [valgrind, mtrace]
---

#### categories of memory error
* memory leak
* memory overflow
* dangling pointers

#### how to detect memory error
* ps -aux, check VSZ, e.g:
  - `ps -aux |grep -E 'VSZ|2892.*firefox' |grep -v grep`
  - `watch -n 5 'ps -aux |grep -E "VSZ|2892.*firefox" |grep -v grep'`

#### static program analysis tools
* [pc-lint](http://www.gimpel.com/html/lintinfo.htm)
* [spint](http://www.splint.org/), a tool for statically checking c programs for  
  vulnerabilities and coding mistakes.

#### detect methods
* mtrace
* valgrind

#### reference
* [内存调试技巧](https://www.ibm.com/developerworks/cn/aix/library/au-memorytechniques.html)
* [techniques for memory debugging](http://www.ibm.com/developerworks/aix/library/au-memorytechniques.html?S_TACT=105AGX52&S_CMP=cn-a-au)
* [C/C++的内存泄漏检测工具Valgrind memcheck的使用经历](http://www.cnblogs.com/lanxuezaipiao/p/3604533.html)
* [如何在linux下检测内存泄漏](https://www.ibm.com/developerworks/cn/linux/l-mleak/)
* [应用 Valgrind 发现 Linux 程序的内存问题 ](https://www.ibm.com/developerworks/cn/linux/l-cn-valgrind/)
* [内存管理内幕](http://www.ibm.com/developerworks/cn/linux/l-memory/)
* [Inside memory management](http://www.ibm.com/developerworks/cn/linux/l-memory/)
* [Understanding Valgrind memory leak reports](http://es.gnu.org/~aleksander/valgrind/valgrind-memcheck.pdf)
* [在 Linux 平台中调试 C/C++ 内存泄漏方法](https://www.ibm.com/developerworks/cn/linux/l-cn-memleak/)
* [代码静态分析工具——splint的学习与使用](http://www.cnblogs.com/bangerlee/archive/2011/09/07/2166593.html)
* [spint](http://www.splint.org/)
* [pc-lint](http://www.gimpel.com/html/lintinfo.htm)
