---
layout: post
title: "linux memory"
description: ""
category: program 
tags: [memory]
---

### process and memory
* how process use memory
  There are 5 kinds of segment datas on process, which is code segment, data segment, 
  BSS segment, heap segment and stack segment.
  - code, use for store execute instruction.
  - data, store global variable, which had been initialized.
  - BSS, uninitialized global variable, all would be setted to zero.
  - heap, memory be allocated dynamic
  - stack, use for store local variable

* how these segments look like
  - |stack address ... heap address ... BSS ... data ... code|
  - stack address is on high address, and code is on low.
  - here is an example to tell the ture  

        #include<stdio.h>
        #include<malloc.h>
        #include<unistd.h>
        int bss_var;
        int data_var0=1;
        int main(int argc,char **argv)
        {
          printf("below are addresses of types of process's mem\n");
          printf("Text location:\n");
          printf("\tAddress of main(Code Segment):%p\n",main);
          printf("____________________________\n");
          int stack_var0=2;
          printf("Stack Location:\n");
          printf("\tInitial end of stack:%p\n",&stack_var0);
          int stack_var1=3;
          printf("\tnew end of stack:%p\n",&stack_var1);
          printf("____________________________\n");
          printf("Data Location:\n");
          printf("\tAddress of data_var(Data Segment):%p\n",&data_var0);
          static int data_var1=4;
          printf("\tNew end of data_var(Data Segment):%p\n",&data_var1);
          printf("____________________________\n");
          printf("BSS Location:\n");
          printf("\tAddress of bss_var:%p\n",&bss_var);
          printf("____________________________\n");
          char *b = sbrk((ptrdiff_t)0);
          printf("Heap Location:\n");
          printf("\tInitial end of heap:%p\n",b);
          brk(b+4);
          b=sbrk((ptrdiff_t)0);
          printf("\tNew end of heap:%p\n",b);
          return 0;
        }
  - output as below  
        below are addresses of types of process's mem
        Text location:
            Address of main(Code Segment):0x400606
        ____________________________
        Stack Location:
            Initial end of stack:0x7fff8b4d1b5c
            new end of stack:0x7fff8b4d1b58
        ____________________________
        Data Location:
            Address of data_var(Data Segment):0x601050
            New end of data_var(Data Segment):0x60104c
        ____________________________
        BSS Location:
            Address of bss_var:0x601058
        ____________________________
        Heap Location:
            Initial end of heap:0x203c000
            New end of heap:0x203c004
* memory space of process
  Linux use virtual memory address to express the process memory space, it make  
  all the process have isolate physic memory space, they will not interfere with  
  each other.
* process memory manager
  Use command `cat /proc/[pid]/maps` can show the memory information of process.
* allocate and recover process memory 
* system physic memory manager

### Reference
* [Linux内存管理](http://www.kerneltravel.net/journal/v/mem.htm)
