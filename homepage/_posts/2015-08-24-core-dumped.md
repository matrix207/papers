---
layout: post
title: "core dumped"
description: ""
category: program 
tags: [debug]
---

### What is coredump

### How to analyse coredump

### Why program coredump
* memory overload
* using thread-unsafe functions on multi-thread
* not using lock when read-write data in multi-thread
* invalid point
* stack overload

### How to use core file
* allow to generate core file
  - `ulimit –c unlimited`
  - `echo /tmp/core.%e.%p > /proc/sys/kernel/core_pattern`
  - `gcc –g core_dump_test.c -o core_dump_test`, must used `-g` to build.
* get core file
  - `./test`
* debug
  - `gdb ./test /tmp/core`
  - `bt`
  - `f 3`
  - `p buffer`

### Signal of core
* `man 7 signal`

### Examples
* write data to protected memory

        #include <stdio.h>
        int main(){
           int i=0;
           scanf("%d",i);
           printf("%d\n",i);
           return 0;
        }

        #include <stdio.h>
        int main(){
           char *p;
           p = NULL;
           *p = 'x';
           printf("%c", *p);
           return 0;
        }

* memory overflow (array overflow, variable type different and so on)

        #include <stdio.h>
        int main(){
            char test[1];
            printf("%c", test[1000000000]);
            return 0;
        }

Access to a not exist address

        #include <stdio.h>
         int main(){
              int b = 10;
              printf("%s\n", b);
              return 0;
        }

        #include <stdio.h>
        #include <string.h>
          int main(){
             char c='c';
             int i=10;
             char buf[100];
             printf("%s", c); //试图把char型按照字符串格式输出，这里的字符会解释成整数，
                                   //再解释成地址，所以原因同上面那个例子
             printf("%s", i); //试图把int型按照字符串输出
             memset(buf, 0, 100);
             sprintf(buf, "%s", c);  试图把char型按照字符串格式转换
             memset(buf, 0, 100);
             sprintf(buf, "%s", i);//试图把int型按照字符串转换
        }

* Other, likes:
  - Initialization when defined point, check point before using.
  - Careful array index
  - check if success when do `pthread_create`, if fail and do `pthread_join` for  
    this thread will occur segment fault

### Auto start gdb when core occur

    #include <stdio.h>
    #include <stdlib.h>
    #include <signal.h>
    #include <string.h>
    #include <sys/types.h>
    #include <unistd.h>

    void dump(int signo)
    {
        char buf[1024];
        char cmd[1024];
        FILE *fh;

        snprintf(buf, sizeof(buf), "/proc/%d/cmdline", getpid());
        if(!(fh = fopen(buf, "r")))
            exit(0);
        if(!fgets(buf, sizeof(buf), fh))
            exit(0);
        fclose(fh);
        if(buf[strlen(buf) - 1] == '\n')
            buf[strlen(buf) - 1] = '\0';
        snprintf(cmd, sizeof(cmd), "gdb %s %d", buf, getpid());
        system(cmd);

        exit(0);
    }

    void dummy_function (void)
    {
        unsigned char *ptr = 0x00;
        *ptr = 0x00;
    }

    int main (void)
    {
        signal(SIGSEGV, &dump);
        dummy_function ();

        return 0;
    }

### Reference
* [coredump简介与coredump原因总结](http://blog.csdn.net/newnewman80/article/details/8173770)
* [C/C++中的段错误（Segmentation fault）](http://www.cnblogs.com/hello--the-world/archive/2012/05/31/2528326.html)
* [段错误bug的调试](http://blog.csdn.net/freshman_fantom_ywj/article/details/6434074)
