---
layout: post
title: "debugging programs with gdb"
description: ""
category: program 
tags: [debug]
---

### Features
* let program run by your order
* set breakpoints and let program stop
* check values when program stop
* change environment of program

### Basic
1. **add `-g` option to compile, e.g: `gcc -g -Wall -o test test.c`**  

2. run
	
		gdb ./test   <--- start gdb debug
		l            <--- list source code
		break 5      <--- break at 5th line
		break fun1   <--- break at func1 (function name)
		info break   <--- show all break info
		r            <--- run ./test
		n            <--- execute next line
		c            <--- continue 
		p i          <--- print value of i
		bt           <--- backtrace of the stack (same as info stack)
		finish       <--- exit function
		q            <--- exit gdb

### Advance
1. modify program running environment  

		(gdb) set args -f -i /tmp/rssp.pid  
		(gdb) show args  
		Argument list to give program being debugged when it is started is "-f -i /tmp/rssp.pid".  

2. debug running programm  

		find PID by command pgrep, then using `gdb <program> PID` start debugging  
		using command `gdb <program>`，then using `attach PID` to start debugging, `detach` to cancel.

3. info command  

		info address -- Describe where symbol SYM is stored  
		info args -- Argument variables of current stack frame  
		info bookmarks -- Status of user-settable bookmarks  
		info breakpoints -- Status of specified breakpoints (all user-settable breakpoints if no argument)  
		info checkpoints -- IDs of currently known checkpoints  
		info classes -- All Objective-C classes  
		info common -- Print out the values contained in a Fortran COMMON block  
		info files -- Names of targets and files being debugged  
		info float -- Print the status of the floating point unit  
		info frame -- All about selected stack frame  
		info functions -- All function names  
		info line -- Core addresses of the code for a source line  
		info locals -- Local variables of current stack frame  
		info macro -- Show the definition of MACRO  
		info macros -- Show the definitions of all macros at LINESPEC  
		info mem -- Memory region attributes  
		info os -- Show OS data ARG  
		info pretty-printer -- GDB command to list all registered pretty-printers  
		info proc -- Show /proc process information about any running process  
		info program -- Execution status of the program  
		info signals -- What debugger does when program gets various signals  
		info source -- Information about the current source file  
		info sources -- Source files in the program  
		info stack -- Backtrace of the stack  
		info symbol -- Describe what symbol is at location ADDR  
		info threads -- Display currently known threads  
		info tracepoints -- Status of specified tracepoints (all tracepoints if no argument)  
		info watchpoints -- Status of specified watchpoints (all watchpoints if no argument)  

4. break command 
	
		break file:linenumber  
		break file:function  
		break *address  
		break ... if <condition>  

		ofcourse, you can clear or disable or enable the breakpoint, 

		clear
		clear <function>
		clear <filename:function>
		clear <linenum>
		clear <filename:linenum>

		delete [breakpoints] [range...]

		disable [breakpoints] [range...]

		enable [breakpoints] [range...]

5. watch  

		watch <expr>
		rwatch <expr>
		awatch <expr>
		info watchpoints

6. catch point  
	catch <event>, event list as below:  

		1).throw
		2).catch
		3).exec
		4).fork
		5).vfork
		6).load
		7).unload

7. condition  

8. signal  

9. thread  

		- info threads 显示当前可调试的所有线程，每个线程会有一个GDB为其分配的ID，后面操  
					   作线程的时候会用到这个ID。 前面有*的是当前调试的线程。
		- thread ID 切换当前调试的线程为指定ID的线程。
		- break thread_test.c:123 thread all 在所有线程中相应的行上设置断点
		- thread apply ID1 ID2 command 让一个或者多个线程执行GDB命令command。 
		- thread apply all command 让所有被调试线程执行GDB命令command。
		- set scheduler-locking off|on|step   
		  估计是实际使用过多线程调试的人都可以发现，在使用step或者continue命令调试当前被  
		  调试线程的时候，其他线程也是同时执行的，怎么只让被调试程序执行呢？通过这个命令  
		  就可以实现这个需求。
		  + off 不锁定任何线程，也就是所有线程都执行，这是默认值。   
		  + on只有当前被调试程序会执行。 
		  + step 在单步的时候，除了next过一个函数的情况(熟悉情况的人可能知道，这其实是一  
			个设置断点然后continue的行为)以外，只有当前线程会执行。

example:


	(gdb) r
	Starting program: /root/thread
	[New Thread 1073951360 (LWP 12900)]
	[New Thread 1082342592 (LWP 12907)]---以下三个为新产生的线程
	[New Thread 1090731072 (LWP 12908)]
	[New Thread 1099119552 (LWP 12909)]

	查看线程：使用info threads可以查看运行的线程。 

	(gdb) info threads
	  4 Thread 1099119552 (LWP 12940)   0xffffe002 in ?? ()
	  3 Thread 1090731072 (LWP 12939)   0xffffe002 in ?? ()
	  2 Thread 1082342592 (LWP 12938)   0xffffe002 in ?? ()
	* 1 Thread 1073951360 (LWP 12931)   main (argc=1, argv=0xbfffda04) at thread.c:21
	(gdb)

	注意，行首的蓝色文字为gdb分配的线程号，对线程进行切换时，使用该该号码，而不是
	上文标出的绿色数字。

	另外，行首的红色星号标识了当前活动的线程

	切换线程：使用 thread THREADNUMBER 进行切换，THREADNUMBER 为上文提到的线程号。
	下例显示将活动线程从 1 切换至 4。 

	(gdb) info threads
	   4 Thread 1099119552 (LWP 12940)   0xffffe002 in ?? ()
	   3 Thread 1090731072 (LWP 12939)   0xffffe002 in ?? ()
	   2 Thread 1082342592 (LWP 12938)   0xffffe002 in ?? ()
	* 1 Thread 1073951360 (LWP 12931)   main (argc=1, argv=0xbfffda04) at thread.c:21
	(gdb) thread 4
	[Switching to thread 4 (Thread 1099119552 (LWP 12940))]#0   0xffffe002 in ?? ()
	(gdb) info threads
	* 4 Thread 1099119552 (LWP 12940)   0xffffe002 in ?? ()
	   3 Thread 1090731072 (LWP 12939)   0xffffe002 in ?? ()
	   2 Thread 1082342592 (LWP 12938)   0xffffe002 in ?? ()
	   1 Thread 1073951360 (LWP 12931)   main (argc=1, argv=0xbfffda04) at thread.c:21
	(gdb)

打印线程信息:

	usage:
		break <linespec> thread <threadno>
		break <linespec> thread <threadno> if ...
	
example:
	(gdb) break frik.c:13 thread 28 if bartab > lim

例子:

	(gdb) r
	Starting program: /home/dennis/work/test/test 
	[Thread debugging using libthread_db enabled]
	Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".
	[New Thread 0x7ffff77f5700 (LWP 5073)]
	thr_fun_1: 9
	[New Thread 0x7ffff6ff4700 (LWP 5074)]
	thr_fun_2: 69
	[New Thread 0x7ffff67f3700 (LWP 5075)]
	 >> func_1
	 >> func_2
	 >> func_3
	 >> func_4
	 >> func_5
	thr_fun_3: 99
	thr_fun_1: 8
	thr_fun_3: 98
	^C
	Program received signal SIGINT, Interrupt.
	0x00007ffff7bc566b in pthread_join (threadid=140737345705728, thread_return=0x0) at pthread_join.c:92
	92	pthread_join.c: No such file or directory.
	(gdb) break main.c:45 thread 4
	Breakpoint 1 at 0x400849: file main.c, line 45.
	(gdb) c
	Continuing.
	thr_fun_3: 97
	thr_fun_1: 7
	thr_fun_2: 68
	[Switching to Thread 0x7ffff67f3700 (LWP 5075)]

	Breakpoint 1, thr_fun_3 (parm=0x0) at main.c:45
	45			sleep(1);
	(gdb) l
	40	void* thr_fun_3 (void *parm)
	41	{
	42		int count = 100;
	43		while (count--) {
	44			printf("%s: %d\n",__FUNCTION__, count);
	45			sleep(1);
	46		}
	47		return NULL;
	48	}
	49	
	(gdb) print count
	$1 = 97
	(gdb) 

10. stack info  

11. memory of code  

	info line test.c:func

	disassemble func

12. view values of running data

	(gdb) p file::variable
	(gdb) p function::variable
	

	(gdb) p *array@len

13. view memory
	examine

14. auto display
	display

15. set print option

16. history

17. view register

	info registers
	info all-registers
	info registers <regname...>

18. change program value
	
	(gdb) print x=4
	(gdb) set var width=47

19. jump


20. run shell command

	(gdb) shell <command string>

21. view all backtrace of all thread: `thread apply all bt`, short type `t a a bt`

22. `set print elements 1024`, if want unlimited, use `set print elements 0`, use
    for print std::string contents. Type `help set print elements` for more 

23. `x/20xb &arr`, print 20 bytes start from address of variable `arr`, help with `help x`

24. `dump binary memory filename start_addr end_addr` dump memory to files
    ref : <https://sourceware.org/gdb/onlinedocs/gdb/Dump_002fRestore-Files.html>

### Reference 
* [用GDB调试程序 1](http://blog.csdn.net/haoel/article/details/2879)
* [用GDB调试程序 2](http://blog.csdn.net/haoel/article/details/2880)
* [用GDB调试程序 3](http://blog.csdn.net/haoel/article/details/2881)
* [用GDB调试程序 4](http://blog.csdn.net/haoel/article/details/2882)
* [用GDB调试程序 5](http://blog.csdn.net/haoel/article/details/2883)
* [用GDB调试程序 6](http://blog.csdn.net/haoel/article/details/2884)
* [用GDB调试程序 7](http://blog.csdn.net/haoel/article/details/2885)
* [GDB调试器基本命令必知必会](http://blog.csdn.net/xiajun07061225/article/details/8960332)
* <http://poormansprofiler.org/hpmp.txt>
