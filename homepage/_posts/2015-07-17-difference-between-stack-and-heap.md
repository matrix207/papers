---
layout: post
title: "difference between stack and heap"
description: ""
category: program 
tags: [memory]
---

### Difference
* stack 
  - allocate memory on stack by operate system
  - Have limit size, and less than 1MB always
* heap 
  - allocate memeory on heap
  - using `malloc` or `new` to allocate memory
  - user and defined the size, and the limit depend on virtual memory size
  - prone to memory fragments

### What is memory fragments
* Internal fragmentation 
  - memory allocation provided memory to program in chunks divisible by 4, 8 or 16
  - if application request 23 bytes, it actually get a chunk of 32 bytes. 
  - Really? How to prove it? And what happen if I using memory not belong to me?
* External fragmentation 
  - cause by `free` called
  - An example, first, you allocate 9 bytes memory, then allocate another 5 bytes,  
    and later you free the first 9 bytes, but now, you want to allocate 10 bytes,  
    this time the first 9 bytes memory can't be used, because it is not enough.  
    Allocation must using continues memory, so it would allocate another 10 bytes  
    behind the 5 bytes position. Now the first block memory is memory fragments.

### How to avoid memory fragments

### Why memory overflow but not error occur?
* sample code
		#include <stdio.h>
		#include <stdlib.h>
		#include <string.h>

		void main()
		{
			// allocate 13 bytes memory on heap
			char* p = (char*)malloc(13);
			char* c_12 = "012345678912";
			char* c_15 = "012345678912345";
			char* c_19 = "0123456789123456789";
			char* c_99 = "aaaaaaaaaaaaaaaaaaaa"
			"aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"
			"aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"
			"aaaaaaaaaaaaa";

			printf("do copy 12 bytes to p\n");
			strncpy(p, c_12, strlen(c_12)+1);
			p[12] = '\0'; printf("p=%s\n", p);

			printf("do copy 15 bytes to p\n");
			strncpy(p, c_15, strlen(c_15)+1);
			p[16] = '\0'; printf("p=%s\n", p);

			printf("do copy 19 bytes to p\n");
			strncpy(p, c_19, strlen(c_19)+1);
			p[20] = '\0'; printf("p=%s\n", p);

			printf("do copy 99 bytes to p\n");
			strncpy(p, c_99, strlen(c_99)+1);
			p[100] = '\0'; printf("p=%s\n", p);

			printf("Hello world\n");
		}

### Reference
* [内存碎片](http://blog.csdn.net/xuzhonghai/article/details/7285821)
* [Fragmentation](https://en.wikipedia.org/wiki/Fragmentation_(computing))
