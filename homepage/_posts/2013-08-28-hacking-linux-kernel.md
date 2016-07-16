---
layout: post
title: "Hacking Linux Kernel"
description: ""
category: kernel
tags: [kernel]
---

##1. Introduce
* How to hack the Linux kernel.
* Stuffs related to Linux kernel development.
* Useful tools for Linux kernel development.
* The developing cycle of Linux kernel.

##2. Where is Linux kernel?
* [Official website](http://www.kernel.org)
* [Bug report](http://bugzilla.kernel.org)
* [Mail archive site](http://lkml.org)

##3. Stable Release 
* **2.6.x** kernels are maintained by Linus Torvalds.
* **2.4.x** kernels are maintained by Marcelo Tosatt.
* **2.2.x** kernels are maintained by Alan Cox.
* **2.0.x** kernels are maintained by David Weinehall.
* **-stable** kernels are maintained by Greg KH.

##4. Patch
* How to write a patch
* Apply the patch 
* Patch format

##5. Mailing lists
* The main list: lkml@vger.kernel.org
* Read the LKML FAQ before you subscribe.
* Linux filesystem development: linux-fsdevel@vger. kernel.org
* linux-netdev: netdev@vger.kernel.org

##6. Advice
* Alan Cox: “Ignore everyone who tells you kernel hacking is hard, special or 
  different. It's a large program, and bug fixing or driver tweaking can be a 
  best starting point. It is however not magic, nor written in a secret language 
  that only deep initiates with beards can read.”
* Be persistent, be patient, please.

##7. module compile
compile dirver "target" for linux-3.10.0-123.el7 :

* Download kernel code kernel-3.10.0-123.el7.src.rpm
* rpm -ivh kernel-3.10.0-123.el7.src.rpm
* cp ~/rpmbuild/SOURCES/linux-3.10.0-123.el7.tar.xz /root/
* tar xvf linux-3.10.0-123.el7.tar.xz
- cd /root/linux-3.10.0-123.el7 
- make oldconfig && make prepare 
- make scripts
- make CONFIG_ISCSI_TARGET=m -C /root/linux-3.10.0-123.el7 M=drivers/target/iscsi

##8. debug
Enable pr_debug macro function:  

* vim /root/linux-3.10.0-123.el7 M=drivers/target/iscsi/Makefile
* Add code list below:(equal to add `#define DEBUG` to the file_name.c)  

		CFLAGS_iscsi_target_parameters.o := -DDEBUG
		CFLAGS_iscsi_target_seq_pdu_list.o := -DDEBUG
		CFLAGS_iscsi_target_tq.o := -DDEBUG
		CFLAGS_iscsi_target_auth.o := -DDEBUG
		CFLAGS_iscsi_target_datain_values.o := -DDEBUG
		CFLAGS_iscsi_target_device.o := -DDEBUG
		CFLAGS_iscsi_target_erl0.o := -DDEBUG
		CFLAGS_iscsi_target_erl1.o := -DDEBUG
		CFLAGS_iscsi_target_erl2.o := -DDEBUG
		CFLAGS_iscsi_target_login.o := -DDEBUG
		CFLAGS_iscsi_target_nego.o := -DDEBUG
		CFLAGS_iscsi_target_nodeattrib.o := -DDEBUG
		CFLAGS_iscsi_target_tmr.o := -DDEBUG
		CFLAGS_iscsi_target_tpg.o := -DDEBUG
		CFLAGS_iscsi_target_util.o := -DDEBUG
		CFLAGS_iscsi_target.o := -DDEBUG
		CFLAGS_iscsi_target_configfs.o := -DDEBUG
		CFLAGS_iscsi_target_stat.o := -DDEBUG
		CFLAGS_iscsi_target_transport.o := -DDEBUG

* Go the this page for more <https://www.kernel.org/doc/local/pr_debug.txt>
* Also, you can using `printk(KERN_DEBUG "LIO: get auth from line 140\n");` to your source code.

##Reference:
* [Linux Kernel](https://www.kernel.org/)
* [Linux Kernel Newbies](http://kernelnewbies.org/Linux_Kernel_Newbies)
* [Hacking Linux Kernel](http://wangcong.org/down/kernel.ppt)
* [The Linux Kernel Module Programming Guide](http://www.tldp.org/LDP/lkmpg/2.6/html/lkmpg.html)
* [The Linux Kernel Module Programming Guide-chinese](http://wangcong.org/articles/lkmpg_cn/index.htm)
* [Linux Kernel Travel](http://www.kerneltravel.net/)
* [Kernel coverage at LWN.net](http://lwn.net/Kernel/)
* [Kernel source](https://github.com/torvalds/linux)
