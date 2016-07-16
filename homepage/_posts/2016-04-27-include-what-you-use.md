---
title: 使用IWYU整理头文件引用
date: 2016-04-27 00:14:26
category: program
tags: [c++]
---

### 0x01 介绍
IWYU 是include what you use的首字母缩写
IWYU 使用clang分析符号的引用。是google的一个项目，它可以给出应该引用和移除
的头文件，但并不能保证 100% 是正确的（当然都是语言特性的原因）。

### 0x02 编译源码包
* 安装clang
* 检查clang的版本并下载对应版本的IWYU源码
  - <https://github.com/include-what-you-use/include-what-you-use/releases>
  ```
dennis@dennis:~/Downloads/build$ clang --version
Ubuntu clang version 3.4-1ubuntu3 (tags/RELEASE_34/final) (based on LLVM 3.4)
Target: x86_64-pc-linux-gnu
Thread model: posix
  ```
  - 当前系统使用的clang版本是3.4
* 编译
  - `tar xvf include-what-you-use-clang_3.4.tar.gz`
  - `mkdir build && cd build`
  - `cmake -G "Unix Makefiles" -DLLVM_PATH=/usr/lib/llvm-3.4 ../include-what-you-use-clang_3.4`
  - `make`

### 0x03 举例
* c++ 源码 mem.cc
```
	#include <fcntl.h>
	#include <signal.h>
	#include <stdio.h>
	//#include <stdlib.h>
	#include <string.h>
	#include <stdint.h>
	#include <sys/mman.h>
	#include <sys/stat.h>
	#include <unistd.h>
	#include <sys/user.h>
	#include <execinfo.h>

	#include <thread>
	using namespace std;

	void g()
	{
		usleep(2000);
		printf("thread one\n");
	}

	void f()
	{
		printf("thread two\n");
	}

	int main(int argc, char* argv[])
	{
		std::thread t1(f);
		std::thread t2(g);
		t1.join();
		t2.join();
		return 0;
	}
```

* 检查, 注意,要使用`-std=c++11`, 因为这里使用c11的线程库
```
	dennis@dennis:~/Downloads/build$ ./include-what-you-use -std=c++11 mem.cc 

	mem.cc should add these lines:

	mem.cc should remove these lines:
	- #include <execinfo.h>  // lines 11-11
	- #include <fcntl.h>  // lines 1-1
	- #include <signal.h>  // lines 2-2
	- #include <stdint.h>  // lines 6-6
	- #include <string.h>  // lines 5-5
	- #include <sys/mman.h>  // lines 7-7
	- #include <sys/stat.h>  // lines 8-8
	- #include <sys/user.h>  // lines 10-10

	The full include-list for mem.cc:
	#include <stdio.h>                      // for printf
	#include <unistd.h>                     // for usleep
	#include <thread>                       // for thread
	---
```

* 修改
```
	//#include <fcntl.h>
	//#include <signal.h>
	#include <stdio.h>
	//#include <stdlib.h>
	//#include <string.h>
	//#include <stdint.h>
	//#include <sys/mman.h>
	//#include <sys/stat.h>
	#include <unistd.h>
	//#include <sys/user.h>
	//#include <execinfo.h>
```

* 再次检查,显示已正确引用或前向声明
```
	dennis@dennis:~/Downloads/build$ ./include-what-you-use -std=c++11 mem.cc 

	(mem.cc has correct #includes/fwd-decls)
```

### 0x04 参考
* [使用 I.W.Y.U 整理头文件引用](http://xinsuiyuer.github.io/blog/2013/10/30/include-what-you-use/)
* [include-what-you-use](https://github.com/include-what-you-use/include-what-you-use)
的头文件，但并不能保证 100% 是正确的，此 wiki Why Include What You Use Is Difficult 描述了其中的难点（当然都是语言特性的原因）。
