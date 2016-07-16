---
layout: post
title: "Rapidly method for clean directory with rsync"
description: ""
category: linux 
tags: [linux]
---

### Methods
* rm
* rsync

### Test
	[dennis@localhost ttt]$ mkdir empty
	[dennis@localhost ttt]$ mkdir tmp/; seq 1 40000 | xargs -I{} touch tmp/file_{} 
	[dennis@localhost ttt]$ mkdir tmp1/; time seq 1 40000 | xargs -I{} touch tmp1/file_{} 

	real	0m28.781s
	user	0m0.361s
	sys	0m3.091s
	[dennis@localhost ttt]$ ls -ld *
	drwxrwxr-x. 2 dennis dennis    4096 Sep 15 09:07 empty
	drwxrwxr-x. 2 dennis dennis 1114112 Sep 15 09:11 tmp
	drwxrwxr-x. 2 dennis dennis 1114112 Sep 15 09:13 tmp1
	[dennis@localhost ttt]$ strace -c rsync -a --delete empty/ tmp/
	% time     seconds  usecs/call     calls    errors syscall
	------ ----------- ----------- --------- --------- ----------------
	 98.91    0.001000         100        10         1 select
	  1.09    0.000011           2         6         3 wait4
	  0.00    0.000000           0        15           read
	  0.00    0.000000           0         5           write
	  0.00    0.000000           0        21         9 open
	  0.00    0.000000           0        20           close
	  0.00    0.000000           0         5         3 stat
	  0.00    0.000000           0        12           fstat
	  0.00    0.000000           0         1           lstat
	  0.00    0.000000           0        25           mmap
	  0.00    0.000000           0        12           mprotect
	  0.00    0.000000           0         6           munmap
	  0.00    0.000000           0         6           brk
	  0.00    0.000000           0        10           rt_sigaction
	  0.00    0.000000           0         1           rt_sigprocmask
	  0.00    0.000000           0         1         1 rt_sigreturn
	  0.00    0.000000           0         1         1 access
	  0.00    0.000000           0         4           socket
	  0.00    0.000000           0         4         4 connect
	  0.00    0.000000           0         2           socketpair
	  0.00    0.000000           0         1           clone
	  0.00    0.000000           0         1           execve
	  0.00    0.000000           0        11           fcntl
	  0.00    0.000000           0         4           getdents
	  0.00    0.000000           0         1           getcwd
	  0.00    0.000000           0         1           chdir
	  0.00    0.000000           0         2           umask
	  0.00    0.000000           0         1           geteuid
	  0.00    0.000000           0         1           getegid
	  0.00    0.000000           0         1           arch_prctl
	  0.00    0.000000           0         2           openat
	------ ----------- ----------- --------- --------- ----------------
	100.00    0.001011                   193        22 total
	[dennis@localhost ttt]$ strace -c rm -rf tmp1/
	% time     seconds  usecs/call     calls    errors syscall
	------ ----------- ----------- --------- --------- ----------------
	 79.25    0.035329           1     40001           unlinkat
	 17.50    0.007800         186        42           getdents
	  3.07    0.001369           7       196           brk
	  0.18    0.000082          21         4           munmap
	  0.00    0.000000           0         2           read
	  0.00    0.000000           0         8         4 open
	  0.00    0.000000           0        10           close
	  0.00    0.000000           0         4         3 stat
	  0.00    0.000000           0         6           fstat
	  0.00    0.000000           0         1           lstat
	  0.00    0.000000           0         1         1 lseek
	  0.00    0.000000           0        11           mmap
	  0.00    0.000000           0         4           mprotect
	  0.00    0.000000           0         1           ioctl
	  0.00    0.000000           0         1         1 access
	  0.00    0.000000           0         1           execve
	  0.00    0.000000           0         9           fcntl
	  0.00    0.000000           0         1           fstatfs
	  0.00    0.000000           0         1           arch_prctl
	  0.00    0.000000           0         2           openat
	  0.00    0.000000           0         1           newfstatat
	------ ----------- ----------- --------- --------- ----------------
	100.00    0.044580                 40307         9 total

### Analyze system call
* rm  
  Do so many system calls, special of unlinkat, it take of 79.25% of time,   

* rsync  

### Analyze source code
* [rsync](http://rsync.samba.org/>)

### Reference
* [How can someone rapidly delete 400,000 files?](http://www.quora.com/How-can-someone-rapidly-delete-400-000-files)
* [为什么rsync能够快速删除400000文件](http://blog.chinaunix.net/uid-10705106-id-3879288.html)
