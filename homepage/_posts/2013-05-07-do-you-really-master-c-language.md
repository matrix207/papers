---
layout: post
title: "Do you really master c language"
description: ""
category: language
tags: [c]
---

##1. Array
1. Two dimensional array:  
If you have an 2-d array like `char buf[50][255] = {0};`, and use it for function 

	void test(char *buf[])
	{
		printf("buf[0] = %s\n", buf[0]);
	}

It would appear warning as below:  

	[dennis@localhost test]$ gcc -o t t.c
	t.c: In function ‘main’:
	t.c:179:2: warning: passing argument 1 of ‘test’ from incompatible pointer type [enabled by default]
	t.c:146:6: note: expected ‘char **’ but argument is of type ‘char (*)[255]’

The correct function format is : `void test(char (*buf)[255])`

2. Array initial:

	/* This code is from strace's source code */
	/* strace is tool use for trace all the system call of linux binary */
	#include <stdio.h>
	#include <stdlib.h>

	#define PTRACE_EVENT_CLONE       3
	#define PTRACE_EVENT_FORK        1
	#define PTRACE_EVENT_VFORK       2
	#define PTRACE_EVENT_VFORK_DONE  5
	#define PTRACE_EVENT_EXEC        4
	#define PTRACE_EVENT_EXIT        6

	#define ARRAY_SIZE(a) (sizeof(a) / sizeof((a)[0]))

	int main(int argc, char* argv[])
	{
		static const char *const event_names[] = {
			[PTRACE_EVENT_CLONE] = "CLONE",
			[PTRACE_EVENT_FORK]  = "FORK",
			[PTRACE_EVENT_VFORK] = "VFORK",
			[PTRACE_EVENT_VFORK_DONE] = "VFORK_DONE",
			[PTRACE_EVENT_EXEC]  = "EXEC",
			[PTRACE_EVENT_EXIT]  = "EXIT",
		};
		
		int i=0; 
		for (i=0; i<ARRAY_SIZE(event_names); i++)
			printf("event_name[%d] = %s\n", i, event_names[i]);
		return 0;
	}

3. Arrays of Length Zero 

    #include <stdlib.h>
    #include <string.h>
     
    struct line {
       int length;
       char contents[0]; // C99的玩法是：char contents[]; 没有指定数组长度
    };
     
    int main(){
        int this_length=10;
        struct line *thisline = (struct line *)
                         malloc (sizeof (struct line) + this_length);
        thisline->length = this_length;
        memset(thisline->contents, 'a', this_length);
        free(thisline);
        return 0;
    }

##2. point

### 2.1 void test(char *a)
	if want to change the value, use this.
### 2.2 void test(char **a)
	if want to allocate memory, use this.

##3. structure

### 3.1 initial by bit
	struct pid_tag {
		unsigned int inactive : 1;
		unsigned int          : 1; /* fill 1 bit */
		unsigned int refcount : 6;
		unsigned int          : 0; /* fill to next word */
        short pid_id;
		struct pid_tag *link;
	}

### 3.2 alignment

	struct st_a {
		int a;
		char b;
		short c;
	};

	struct st_b {
		char b;
		short c;
		int a;
	};

	struct st_c {
		char b;
		int a;
		short c;
	};

	the value of sizeof(st_a) is 8  (address: 0 1 2 3 | 4 . | 6 7)
	the value of sizeof(st_b) is 8  (address: 0 . | 2 3 | 4 5 6 7)
	the value of sizeof(st_c) is 12 (address: 0 . . . | 4 5 6 7 | 8 9 . .)

Therefore, you should careful for doing structure copy, and socket programming.


##4. Function

# For more : 
* [comp.lang.c Frequently Asked Questions](http://c-faq.com/index.html)
* [C-FAQ](ftp://rtfm.mit.edu/pub/usenet-by-group/news.answers/C-faq/faq)
* [Top votes of c questions on stackoverflow](http://stackoverflow.com/questions/tagged/c?sort=votes&pagesize=15)
* [如何学好C语言](http://coolshell.cn/articles/4102.html)
* [C语言的谜题](http://coolshell.cn/articles/945.html)
* [谁说C语言很简单？](http://coolshell.cn/articles/873.html)
* [Why do all these crazy function pointer definitions all work? What is really going on?](http://stackoverflow.com/questions/6893285/why-do-all-these-crazy-function-pointer-definitions-all-work-what-is-really-goi)
* [small c project](http://programmers.stackexchange.com/questions/62502/small-c-projects)
* [结构体内存对齐](http://blog.csdn.net/zhaori/article/details/7659268)
* [二维数组做函数参数的问题](http://bbs.chinaunix.net/thread-480691-1-1.html)
* [Arrays of Length Zero](http://gcc.gnu.org/onlinedocs/gcc/Zero-Length.html)
* [C语言结构体里的成员数组和指针](http://coolshell.cn/articles/11377.html)
