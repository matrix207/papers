---
layout: post
title: "stack trace when program crash"
description: ""
category: program
tags: [stack,crash]
---

#### Source

    #include <stdio.h>
    #include <execinfo.h>
    #include <signal.h>
    #include <stdlib.h>
    #include <unistd.h>

    void handler(int sig) {
        void *array[10];
        size_t size;

        // get void*'s for all entries on the stack
        size = backtrace(array, 10);

        // print out all the frames to stderr
        fprintf(stderr, "Error: signal %d:\n", sig);
        backtrace_symbols_fd(array, size, STDERR_FILENO);
        exit(1);
    }

    void baz() {
        int *foo = (int*)-1; // make a bad pointer
        printf("%d\n", *foo);       // causes segfault
    }

    void bar() { baz(); }
    void foo() { bar(); }

    int main(int argc, char **argv) {
        signal(SIGSEGV, handler);   // install our handler
        foo(); // this will call foo, bar, and baz.  baz segfaults.
    }

#### Compile and test

    [dennis@localhost code]$ gcc -g -rdynamic -o t stacktrace.c
    [dennis@localhost code]$ ./t
    Error: signal 11:
    ./t(handler+0x1c)[0x400a32]
    /lib64/libc.so.6(+0x34960)[0x7fa03d30d960]
    ./t(baz+0x14)[0x400a8b]
    ./t(bar+0xe)[0x400aae]
    ./t(foo+0xe)[0x400abe]
    ./t(main+0x28)[0x400ae8]
    /lib64/libc.so.6(__libc_start_main+0xf0)[0x7fa03d2f8fe0]
    ./t[0x400949]

#### Reference
* <http://stackoverflow.com/questions/77005/how-to-generate-a-stacktrace-when-my-gcc-c-app-crashes>
