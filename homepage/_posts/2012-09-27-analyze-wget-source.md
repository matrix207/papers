---
layout: post
title: "Analyze wget source"
description: "Analyze wget source"
category: source
tags: [wget]
---

### Log of study wget source code###


__2012-9-27 00:04:30__

1. config  
	`./configure`
2. compile  
	`make`
3. install  
	`make install`
4. uninstall  
	`make uninstall`

compile log: (on virtualbox os ubuntu)  

	dennis@dennis-VBox:~/project/wget/wget-1.9$ ./configure
	dennis@dennis-VBox:~/project/wget/wget-1.9$ ls
	dennis@dennis-VBox:~/project/wget/wget-1.9$ make
	// Error message: autoconf: Command not found
	// install autoconf
	dennis@dennis-VBox:~/project/wget/wget-1.9$ sudo apt-get install autoconf
	dennis@dennis-VBox:~/project/wget/wget-1.9$ make
	// Error message: makeinfo: Command not found
	// install texinfo
	dennis@dennis-VBox:~/project/wget/wget-1.9$ sudo apt-get install texinfo
	dennis@dennis-VBox:~/project/wget/wget-1.9$ make
	
	Binary file "wget" will be generated at wget-1.9/src/

_todo_  
	analyze Makefile


__2012-9-26 00:06:18__

1. [Home](http://www.gnu.org/software/wget/)
2. [manual site](http://www.gnu.org/software/wget/manual/)
3. manual generation script [gendocs.sh](http://savannah.gnu.org/cgi-bin/viewcvs/~checkout~/texinfo/texinfo/util/gendocs.sh)
4. [pdf format manual](http://www.gnu.org/software/wget/manual/wget.pdf)

_todo_  
	compile wget source code

