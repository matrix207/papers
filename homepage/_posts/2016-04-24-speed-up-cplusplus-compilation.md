---
title: speed up c++ compilation
date: 2016-04-24 16:07:15
category: language
tags: c++
---

### How c++ compilor work?

### Speed up tips
* check code
  - Don't include not needed headers, use forward declaration for class if possible
  - Pimpl (pointer to implementation), interface and implement do their job clear
  - Reduce interdependency
  - Clean not needed headers
  - inline and template
  - Guard Conditions
* other tips
  - PCH (Previous Compile Header)
  - Unity Build
  - ccache
* compile resource
  - parallel compile, compile multiple modules at the same times
  - use more faster computer
  - distribute compile

### Reference
* [如何加快C++代码的编译速度](www.cnblogs.com/baiyanhuang/archive/2010/01/17/1730717.html)
* <http://stackoverflow.com/questions/373142/what-techniques-can-be-used-to-speed-up-c-compilation-times>
* [怎样削减C++代码间依赖](https://segmentfault.com/a/1190000002790854)
* [为什么C++编译速度比Java慢得多？](https://www.zhihu.com/question/42964826)
* [Walter Bright on C++ Compilation Speed](https://www.reddit.com/comments/r9p4c)
* [C++ Compilation Speed](http://www.drdobbs.com/cpp/c-compilation-speed/228701711)
* [How to trick C/C++ compilers into generating terrible code?](http://www.futurechips.org/tips-for-power-coders/how-to-trick-cc-compilers-into-generating-terrible-code.html)
