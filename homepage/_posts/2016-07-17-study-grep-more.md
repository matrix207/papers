---
layout: post
title: grep工作原理
date: 2016-07-17 00:58:00
category: program
tags: [tools]
---

### 0x01 介绍
GNU grep

### 0x02 编译源码包
* source code `git clone git://git.savannah.gnu.org/grep.git`

### 0x03 实现分析
* GNU grep 不会检查每个字节
* 使用更少的指令检查数据
* 显示结果设计，颜色高亮

### 0x04 字符串搜索算法比较
* Knuth-Morris-Pratt算法 (简称KMP算法)
是以三个发明者命名，其中Knuth是指Donald Knuth, 《计算机程序设计艺术》的作者
* Boyer-Moore算法 (也称BM算法)

### 0x05 参考
* [GNU Grep](https://www.gnu.org/software/grep/)
* [GNU Grep Development](https://www.gnu.org/software/grep/devel.html)
* [字符串匹配的KMP算法](http://www.ruanyifeng.com/blog/2013/05/Knuth–Morris–Pratt_algorithm.html)
* [字符串匹配的Boyer-Moore算法](http://www.ruanyifeng.com/blog/2013/05/boyer-moore_string_search_algorithm.html)
