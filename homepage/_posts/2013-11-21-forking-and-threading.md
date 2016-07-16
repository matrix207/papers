---
layout: post
title: "Forking and Threading"
description: ""
category: language
tags: [c++]
---

##Why?

Sometimes, wo need to write some application, which should do multi task, this 
time fork thread or multithread will show in our brain. But which one should we 
need exactly, I think we should known them more before make decision.

##what?

History:


Linkers: 

* <http://blog.csdn.net/Solstice/article/details/5307710>
* <http://stackoverflow.com/questions/16354460/forking-vs-threading>
* <http://stackoverflow.com/questions/2483041/what-is-the-difference-between-fork-and-thread>
* <http://guogoul.com/2008/03/23/process/>
* <http://blog.csdn.net/blade2001/article/details/6790890>


##Forking vs threading

**From:** <http://www.geekride.com/fork-forking-vs-threading-thread-linux-kernel/>

So, finally after long time, i am able to figure out the difference between forking and threading :)

When i have been surfing around, i see a lots of threads/questions regarding forking and threading, lots of queries which one should be used in the applications. So i wrote this post which could clarify the difference between these two based on which you could decide what you want to use in your application/scripts.

### What is Fork/Forking:

Fork is nothing but a new process that looks exactly like the old or the parent process but still it is a different process with different process ID and having  it’s own memory. Parent process creates a separate address space for child. Both parent and child process possess the same code segment, but execute independently from each other.

The simplest example of forking is when you run a command on shell in unix/linux. Each time a user issues a command, the shell forks a child process and the task is done.

When a fork system call is issued, a copy of all the pages corresponding to the parent process is created, loaded into a separate memory location by the OS for the child process, but in certain cases, this is not needed. Like in ‘exec’ system calls, there is not need to copy the parent process pages, as execv replaces the address space of the parent process itself.
Few things to note about forking are:

* The child process will be having it’s own unique process ID.
* The child process shall have it’s own copy of parent’s file descriptor.
* File locks set by parent process shall not be inherited by child process.
* Any semaphores that are open in the parent process shall also be open in the child process.
* Child process shall have it’s own copy of message queue descriptors of the parents.
* Child will have it’s own address space and memory.

Fork is universally accepted than thread because of the following reasons:

* Development is much easier on fork based implementations.
* Fork based code a more maintainable.
* Forking is much safer and more secure because each forked process runs in its own virtual address space. If one process crashes or has a buffer overrun, it does not affect any other process at all.
* Threads code is much harder to debug than fork.
* Fork are more portable than threads.
* Forking is faster than threading on single cpu as there are no locking over-heads or context switching.

Some of the applications in which forking is used are: telnetd(freebsd), vsftpd, proftpd, Apache13, Apache2, thttpd, PostgreSQL.

Pitfalls in Fork:

* In fork, every new process should have it’s own memory/address space, hence a longer startup and stopping time.
* If you fork, you have two independent processes which need to talk to each other in some way. This inter-process communication is really costly.
* When the parent exits before the forked child, you will get a ghost process. That is all much easier with a thread. You can end, suspend and resume threads from the parent easily. And if your parent exits suddenly the thread will be ended automatically.
* In-sufficient storage space could lead the fork system to fail.

### What are Threads/Threading:

Threads are Light Weight Processes (LWPs). Traditionally, a thread is just a CPU (and some other minimal state) state with the process containing the remains (data, stack, I/O, signals). Threads require less overhead than “forking” or spawning a new process because the system does not initialize a new system virtual memory space and environment for the process. While most effective on a multiprocessor system where the process flow can be scheduled to run on another processor thus gaining speed through parallel or distributed processing, gains are also found on uniprocessor systems which exploit latency in I/O and other system functions which may halt process execution.

Threads in the same process share:

* Process instructions
* Most data
* open files (descriptors)
* signals and signal handlers
* current working directory
* User and group id

Each thread has a unique:

* Thread ID
* set of registers, stack pointer
* stack for local variables, return addresses
* signal mask
* priority
* Return value: errno

Few things to note about threading are:

* Thread are most effective on multi-processor or multi-core systems.
* For thread – only one process/thread table and one scheduler is needed.
* All threads within a process share the same address space.
* A thread does not maintain a list of created threads, nor does it know the thread that created it.
* Threads reduce overhead by sharing fundamental parts.
* Threads are more effective in memory management because they uses the same memory block of the parent instead of creating new.

Pitfalls in threads:

* Race conditions: The big loss with threads is that there is no natural protection from having multiple threads working on the same data at the same time without knowing that others are messing with it. This is called race condition. While the code may appear on the screen in the order you wish the code to execute, threads are scheduled by the operating system and are executed at random. It cannot be assumed that threads are executed in the order they are created. They may also execute at different speeds. When threads are executing (racing to complete) they may give unexpected results (race condition). Mutexes and joins must be utilized to achieve a predictable execution order and outcome.
* Thread safe code: The threaded routines must call functions which are “thread safe”. This means that there are no static or global variables which other threads may clobber or read assuming single threaded operation. If static or global variables are used then mutexes must be applied or the functions must be re-written to avoid the use of these variables. In C, local variables are dynamically allocated on the stack. Therefore, any function that does not use static data or other shared resources is thread-safe. Thread-unsafe functions may be used by only one thread at a time in a program and the uniqueness of the thread must be ensured. Many non-reentrant functions return a pointer to static data. This can be avoided by returning dynamically allocated data or using caller-provided storage. An example of a non-thread safe function is strtok which is also not re-entrant. The “thread safe” version is the re-entrant version strtok_r.

Advantages in threads:

* Threads share the same memory space hence sharing data between them is really faster means inter-process communication (IPC) is real fast.
* If properly designed and implemented threads give you more speed because there aint any process level context switching in a multi threaded application.
* Threads are really fast to start and terminate.

Some of the applications in which threading is used are: MySQL, Firebird, Apache2, MySQL 323

### FAQs:

1. Which should i use in my application ?
Ans: That depends on a lot of factors. Forking is more heavy-weight than threading, and have a higher startup and shutdown cost. Interprocess communication (IPC) is also harder and slower than interthread communication. Actually threads really win the race when it comes to inter communication. Conversely, whereas if a thread crashes, it takes down all of the other threads in the process, and if a thread has a buffer overrun, it opens up a security hole in all of the threads.
which would share the same address space with the parent process and they only needed a reduced context switch, which would make the context switch more efficient.

2. Which one is better, threading or forking ?
Ans: That is something which totally depends on what you are looking for. Still to answer, In a contemporary Linux (2.6.x) there is not much difference in performance between a context switch of a process/forking compared to a thread (only the MMU stuff is additional for the thread). There is the issue with the shared address space, which means that a faulty pointer in a thread can corrupt memory of the parent process or another thread within the same address space.

3. What kinds of things should be threaded or multitasked?
Ans: If you are a programmer and would like to take advantage of multithreading, the natural question is what parts of the program should/ should not be threaded. Here are a few rules of thumb (if you say “yes” to these, have fun!):

* Are there groups of lengthy operations that don’t necessarily depend on other processing (like painting a window, printing a document, responding to a mouse-click, calculating a spreadsheet column, signal handling, etc.)?
* Will there be few locks on data (the amount of shared data is identifiable and “small”)?
* Are you prepared to worry about locking (mutually excluding data regions from other threads), deadlocks (a condition where two COEs have locked data that other is trying to get) and race conditions (a nasty, intractable problem where data is not locked properly and gets corrupted through threaded reads & writes)?
* Could the task be broken into various “responsibilities”? E.g. Could one thread handle the signals, another handle GUI stuff, etc.?

### Conclusions:

* Whether you have to use threading or forking, totally depends on the requirement of your application.
* Threads more powerful than events, but power is not something which is always needed.
* Threads are much harder to program than forking, so only for experts.
* Use threads mostly for performance-critical applications.

### References:

* <http://en.wikipedia.org/wiki/Fork_(operating_system)>
* <http://tldp.org/FAQ/Threads-FAQ/Comparison.html>
* <http://www.yolinux.com/TUTORIALS/LinuxTutorialPosixThreads.html>
* <http://linas.org/linux/threads-faq.html>

