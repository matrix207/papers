---
layout: post
title: "How to export a specify full directory by shell file"
description: ""
category: language 
tags: [shell]
---

### Requirement
* project files

	  myprj
		  build
			env.sh
			Makefile
			submake
			  MakeFlagTest1
			  MakeFlagTest2
		  doc
			readme.doc
		  src
			a.h
			a.c
			b.h
			b.c
			main.c
		  test
			test.c

* In env.sh, need to export some environment variable, how to implement?  
  If add hard code, it will looks like : `export PRJ_HOME=/home/dennis/myprj`

* Using case
  - cd /home/dennis; source myprj/build/env.sh
  - cd /home/dennis; source ./myprj/build/env.sh
  - cd /home/dennis; source ~/myprj/build/env.sh
  - cd /home/dennis/myprj/build; source env.sh
  - cd /home/dennis/myprj/build; source ./env.sh
  - cd /home/dennis/myUT/src; source ../../myprj/build/env.sh
  - cd /home/dennis/myUT/src; source ~/myprj/build/env.sh

### Test 1, using `export PRJ_HOME=$(dirname $(dirname $0))`
* Failure case, when on suse release version, will failed to do `dirname $0`,  
  you will find that `echo $0` output `-bash`, not `bash` expected.

### Test 2, using `export PRJ_HOME=$(dirname $(dirname ${BASH_SOURCE[0]))`
* Failure case, when using relative directory.

### Other test

	echo "working directory is " $(pwd)
	echo "BASH_SOURCE is " ${BASH_SOURCE}
	echo "BASH_SOURCE[0] is " ${BASH_SOURCE[0]}
	echo "dirname BASH_SOURCE[0] is " $(dirname ${BASH_SOURCE[0]})
	echo "2 dirname BASH_SOURCE[0] is " $(dirname $(dirname ${BASH_SOURCE[0]}))
	PARENT_DIR=$(dirname $(dirname ${BASH_SOURCE[0]}))
	echo "parent dir is " ${PARENT_DIR}
	export GMDB_HOME=$(pwd ${PARENT_DIR})

### TODO, find the effective method
* If we can get the bash file name?
* How to translate relative directory to be full directory?

### Reference
* [Difference of source , ./ , sh, bash](http://blog.sina.com.cn/s/blog_60d6fadc01013k0k.html)
