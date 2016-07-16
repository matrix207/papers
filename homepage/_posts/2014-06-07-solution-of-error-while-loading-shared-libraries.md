---
layout: post
title: "solution of error while loading shared libraries"
description: ""
category: program 
tags: [c++, protobuf]
---

After I finish compile and install protobuf on my machine, then I flow the tutorial 
to compile the sample code, but after that I can not run it, the error message is 
as below: 

	[dennis@localhost google-code-sample]$ vim addressbook.proto 
	[dennis@localhost google-code-sample]$ ls
	addressbook.proto
	[dennis@localhost google-code-sample]$ protoc -I=./ --cpp_out=./ ./addressbook.proto 
	[dennis@localhost google-code-sample]$ ls
	addressbook.pb.cc  addressbook.pb.h  addressbook.proto
	[dennis@localhost google-code-sample]$ vim pb_sample.cc
	[dennis@localhost google-code-sample]$ ls
	addressbook.pb.cc  addressbook.pb.h  addressbook.proto pb_sample.cc
	[dennis@localhost google-code-sample]$ c++ addressbook.pb.cc pb_sample.cc -o pb_sample -lprotobuf
	[dennis@localhost google-code-sample]$ ls
	addressbook.pb.cc  addressbook.pb.h  addressbook.proto  pb_sample  pb_sample.cc
	[dennis@localhost google-code-sample]$ ./pb_sample 
	./pb_sample: error while loading shared libraries: libprotobuf.so.9: cannot open shared object file: No such file or directory

But I find the library is exist on /usr/local/lib

	[dennis@localhost protobuf]$ ls /usr/local/lib/libproto* -l
	-rw-r--r--. 1 root root 19747400 Jun  7 11:49 /usr/local/lib/libprotobuf.a
	-rwxr-xr-x. 1 root root      968 Jun  7 11:49 /usr/local/lib/libprotobuf.la
	-rw-r--r--. 1 root root  2065712 Jun  7 11:49 /usr/local/lib/libprotobuf-lite.a
	-rwxr-xr-x. 1 root root     1003 Jun  7 11:49 /usr/local/lib/libprotobuf-lite.la
	lrwxrwxrwx. 1 root root       25 Jun  7 11:49 /usr/local/lib/libprotobuf-lite.so -> libprotobuf-lite.so.9.0.0
	lrwxrwxrwx. 1 root root       25 Jun  7 11:49 /usr/local/lib/libprotobuf-lite.so.9 -> libprotobuf-lite.so.9.0.0
	-rwxr-xr-x. 1 root root   982744 Jun  7 11:49 /usr/local/lib/libprotobuf-lite.so.9.0.0
	lrwxrwxrwx. 1 root root       20 Jun  7 11:49 /usr/local/lib/libprotobuf.so -> libprotobuf.so.9.0.0
	lrwxrwxrwx. 1 root root       20 Jun  7 11:49 /usr/local/lib/libprotobuf.so.9 -> libprotobuf.so.9.0.0
	-rwxr-xr-x. 1 root root  8343446 Jun  7 11:49 /usr/local/lib/libprotobuf.so.9.0.0
	-rw-r--r--. 1 root root 23573484 Jun  7 11:49 /usr/local/lib/libprotoc.a
	-rwxr-xr-x. 1 root root      984 Jun  7 11:49 /usr/local/lib/libprotoc.la
	lrwxrwxrwx. 1 root root       18 Jun  7 11:49 /usr/local/lib/libprotoc.so -> libprotoc.so.9.0.0
	lrwxrwxrwx. 1 root root       18 Jun  7 11:49 /usr/local/lib/libprotoc.so.9 -> libprotoc.so.9.0.0
	-rwxr-xr-x. 1 root root  8689345 Jun  7 11:49 /usr/local/lib/libprotoc.so.9.0.0

How the sytem know Where the libraries is location? 

	[dennis@localhost google-code-sample]$ ls /etc/ld.so.c* -l
	-rw-r--r--. 1 root root 141410 Jun  7 12:49 /etc/ld.so.cache
	-rw-r--r--. 1 root root     43 Jun  7 12:48 /etc/ld.so.conf

	/etc/ld.so.conf.d:
	total 48
	-rw-r--r--. 1 root root 17 Sep  8  2012 atlas-x86_64.conf
	-r--r--r--. 1 root root 63 Dec  3  2013 kernel-3.11.10-100.fc18.x86_64.conf
	-r--r--r--. 1 root root 63 Nov  4  2013 kernel-3.11.7-100.fc18.x86_64.conf
	-r--r--r--. 1 root root 63 Nov 21  2013 kernel-3.11.9-100.fc18.x86_64.conf
	-rw-r--r--. 1 root root 17 May  3  2013 libiscsi-x86_64.conf
	-rw-r--r--. 1 root root 16 May 29  2013 llvm-x86_64.conf
	-rw-r--r--. 1 root root 17 Dec 10 23:50 mysql-x86_64.conf
	-rw-r--r--. 1 root root 22 Jul 22  2012 qt-x86_64.conf
	-rw-r--r--. 1 root root 24 Feb 26  2013 tracker-x86_64.conf
	-rw-r--r--. 1 root root 15 Nov 25  2013 wine-32.conf
	-rw-r--r--. 1 root root 17 Nov 25  2013 wine-64.conf
	-rw-r--r--. 1 root root 21 Dec 10 03:28 xulrunner-64.conf

	[dennis@localhost google-code-sample]$ cat /etc/ld.so.conf
	include ld.so.conf.d/*.conf

**Solution**, add the library directory to config file, then update 

	[dennis@localhost google-code-sample]$ su -c 'echo "/usr/local/lib" >> /etc/ld.so.conf'
	Password: 
	[dennis@localhost google-code-sample]$ cat /etc/ld.so.conf
	include ld.so.conf.d/*.conf
	/usr/local/lib
	[dennis@localhost google-code-sample]$ su -c 'ldconfig'
	Password: 

right now, you will see that the timestamp of cache file is new

	[dennis@localhost google-code-sample]$ ls /etc/ld.so.cache -l
	-rw-r--r--. 1 root root 141410 Jun  7 13:07 /etc/ld.so.cache

Reference: 

[error while loading shared libraries: xxx.so.x错误的原因和解决办法](http://blog.csdn.net/sahusoft/article/details/7388617)
