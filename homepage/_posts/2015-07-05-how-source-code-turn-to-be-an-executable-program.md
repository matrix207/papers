---
layout: post
title: "How source code turn to be an executable program"
description: ""
category: language
tags: [c++]
---

### What is an executable program
* On linux, the format of executable program is ELF (`Executable and Linking Format`)
* members of executable program
  - text segment
  - data segment
  - bss segment

### How source code be compiled

### Relation of source code, object file, library file, bin file

### How bin file to load library
* dynamic library
* static library

### Example
* Hello world, a.c

		#include <stdio.h>
		int main()
		{
			printf ("Hello world\n");
		}

* compile

		[dennis@localhost ~]$ gcc a.c
		[dennis@localhost ~]$ objdump -f a.out 

		a.out:     file format elf64-x86-64
		architecture: i386:x86-64, flags 0x00000112:
		EXEC_P, HAS_SYMS, D_PAGED
		start address 0x0000000000400440
		[dennis@localhost ~]$ file a.out
		a.out: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.32, BuildID[sha1]=7396e4d6e7100d1536548d05ba91a67255bf9592, not stripped
		[dennis@localhost ~]$ size a.out
		   text	   data	    bss	    dec	    hex	filename
		   1226	    548	      4	   1778	    6f2	a.out
		[dennis@localhost ~]$ readelf -h a.out
		ELF Header:
		  Magic:   7f 45 4c 46 02 01 01 00 00 00 00 00 00 00 00 00 
		  Class:                             ELF64
		  Data:                              2's complement, little endian
		  Version:                           1 (current)
		  OS/ABI:                            UNIX - System V
		  ABI Version:                       0
		  Type:                              EXEC (Executable file)
		  Machine:                           Advanced Micro Devices X86-64
		  Version:                           0x1
		  Entry point address:               0x400440
		  Start of program headers:          64 (bytes into file)
		  Start of section headers:          4504 (bytes into file)
		  Flags:                             0x0
		  Size of this header:               64 (bytes)
		  Size of program headers:           56 (bytes)
		  Number of program headers:         9
		  Size of section headers:           64 (bytes)
		  Number of section headers:         30
		  Section header string table index: 27
		[dennis@localhost ~]$ readelf -S a.out

### Reference
* [linux可执行文件格式](http://bbs.pediy.com/showthread.php?t=81284.html)
* [Linux下可执行文件格式详解](http://blog.csdn.net/topasstem8/article/details/38730971)
