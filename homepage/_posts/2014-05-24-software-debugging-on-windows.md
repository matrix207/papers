---
layout: post
title: "Software debugging on windows"
description: ""
category: program
tags: [debug]
---

#### User mode and kernel mode debugger

#### Basic Debug methods
* Run with IDE directory, and use breakpoint and single step to check the problem
* Using OutputDebugString, and capture the message with debugview
* Write debugging information to a log file

#### Advanced method
* dump file
* windbg

#### Memory leak detect tools

#### Reference
+ [WDK and WinDbg](http://msdn.microsoft.com/en-us/windows/hardware/hh852365)
+ [Crash Dump Analysis](http://msdn.microsoft.com/en-us/library/windows/desktop/ee416349(v=vs.85).aspx)
+ [Windows调试工具入门 1](http://www.cnitblog.com/cc682/archive/2008/11/27/51945.html)
+ [Windows调试工具入门 2](http://www.cnitblog.com/cc682/archive/2008/12/03/52182.html)
+ [Windows调试工具入门 3](http://www.cnitblog.com/cc682/archive/2008/12/13/52566.html)
+ [Windows程序员进阶系列-软件调试](http://blog.csdn.net/ithzhang/article/category/1338390)
+ [Windows程序员进阶应该看的那些书](http://blog.csdn.net/ithzhang/article/details/8545821)
