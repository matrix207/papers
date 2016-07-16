---
layout: post
title: "GNU Linux program"
description: ""
category: linux 
tags: [linux]
---

---

### Part 1 Linux program tools

#### GNU cc

    gcc myapp.c -L/home/fred/lib -I/home/fred/include -lnew -o myapp

    gcc cursesapp.c -lncurse -static -o cursesapp

    /*
     * pisqrt.c - Calculate the square of PI 100,000,000
     * times
     */
    #include <stdio.h>
    #include <math.h>

    int main(void)
    {
        double pi = M_PI; /* Defined in <math.h> */
        double pisqrt;
        long i;

        for (i=0; i<10000000; ++i) {
            pisqrt = sqrt(pi);
        }

        return 0;
    }

#### GNU make

    auto variable
    $@
    $<
    $^
    $?
    $(@D)
    $(@F)

#### GNU autoconf

    configure.in
    AC_INIT(unique_file_in_source_dir)
    AC_OUTPUT([file...[,extra_cmds[,init_cmds]]])

    AC_INIT
        Test program
        Test function library
        Test header
        Test type defined
        Test structure
        Test compile
        Test library function
        Test system call
    AC_OUTPUT

    autoscan
    ifnames

#### patch

    diff
    patch

    diff -c sigrot.1 sigrot.2 > sigrot.path
    patch -p0 < sigrot.patch

#### GDB

#### assert

    #include <assert.h>
    void assert (int expression);

#### macro

    __LINE__
    __FILE__
    __FUNCTION__

#### system header and error

    stdlib.h	void abort(void);
    stdlib.h	void exit(int status);
    stdlib.h	int atexit(void (*fcn) (void));
    stdio.h		void perror(const char *s);
    string.h	char *strerror(int errnum);
    errno.h		int errno;

    void clearerr(FILE *stream);
    int feof(FILE *stream);
    int ferror(FILE *stream);

#### system log

    klogd
    syslogd

    #include <syslog.h>
    void syslog(int priority, char *format, ...);

    syslog(LOG_WARNING | LOG_USER, "unable to open file %s *** %m\n", fname);

    for shell:
    logger [-s] [-f file] [-p pri] [-t tag] [-u socket] [message ...]

#### dynamic library

    nm [options] file
    ar {dmpgrtx} [member] archive files ...
    ldd [options] file
    ldconfig [options] [libs]

    gcc -fPIC -g -c liberrr.c -o liberr.o
    gcc -g -shared -Wl,-soname,liberr.so -o liberr.so.1.0.0 liberr.o -lc
    ln -s liberr.so.1.0.0 liberr.so.1

    void *dlopen(const char *filename, int flag);
    void *dlsym(void *handle, char *symbol);
    const char *dlerror(void);
    int dlclose(void *handle);

    gcc -g -Wall dltest.c -o dltest -ldl

---

### Part 2 System Program

#### File I/O

    open
    create
    close
    read
    write
    ftruncate
    lseek
    fsync
    fstat
    fchown
    fchmod
    flock
    fcntl
    dup
    dup2
    slect
    ioctl

    fopen
    freopen
    fclose

    feof
    ferror
    clearer
    fileno

    printf
    fprintf
    sprintf
    snprintf

    vprintf
    vfprintf
    vsprintf
    vsnprintf

    scanf
    fscanf
    sscanf

    vscanf
    vsscanf
    vfscanf

    fseek
    ftell
    fgetpos
    fsetpos
    rewind

    fflush
    setbuf
    setbuffer
    setlinebuf
    setvbuf

    remove
    rename

    tmpfile
    tmpnam

    mkstemp

    getcwd
    chdir
    fchdir

    mkdir
    rmdir

    opendir
    readdir
    rewinddir
    closedir

#### Process control

    system
    fork
    exec
    popen
    pclose

    wait
    waitpid

    exit
    abort
    kill

    alarm
    pause

    sigemptyset
    sigfillset
    sigaddset
    sigdelset
    sigismember

    sigprocmask
    sigaction
    sgpending

    sched_setscheduler
    sched_getscheduler
    sched_get_priority_max
    sched_get_priority_min
    getpriority
    setpriority
    nice

#### Thread

    _clone

    pthread_create
    pthread_exit
    pthread_join
    pthread_detach
    pthread_atfork
    pthread_cancel
    pthread_setcancelstate
    pthread_setcanceltype
    pthread_testcancel

    pthread_cleanup_push
    pthread_cleanup_pop
    pthread_cleanup_push_defer_np
    pthread_cleanup_pop_restore_np

    pthread_cond_signal
    pthread_cond_broadcast
    pthread_cond_wait
    pthread_cond_timewait
    pthread_cond_destroy

    pthread_equal

    pthread_attr_

    pthread_mutex_

#### memory manager

    malloc
    calloc
    realloc
    free

    mmap
    mumap
    msync
    mprotect
    mlock
    munlock
    mlockall
    munlockall

---

#### Part 3 IPC and Network program

#### IPC

    pipe
    popen
    pclose

    mkfifo

    shmget
    shmat
    shmdt

    msgget
    msgsnd
    msgrcv
    msgctl

    semget
    semop
    semctl

    setsid

    openlog
    closelog
    syslog


#### Netwrok

    socket
    bind
    listen
    connect
    accept
    read
    write
    recv_from
    send_to

    sendfile
    close

    netstat 
    tcpdump

    multicast

---

### Part 4 GUI

    terminal

    ncurses

    x windows

    GTK+ GUI

    Qt GUI

    OpenGL Mesa 3D

---

### Part 5 Special program technology

#### GNU Bash

#### Device Driver

---

### Part 6 

#### RPM 

#### Licence
1. MIT
2. BSD
3. GNU

---

### Reference 
+ [GNU Linux program]()
+ [Advanced linux program](http://www.advancedlinuxprogramming.com/alp-folder/advanced-linux-programming.pdf)

