---
layout: post
title: "ssd cache and implementation"
description: ""
category: storage 
tags: [linux, cache]
---

#### Introduce
* SSD缓存技术
  - SSD的特性是读速高于写，因此在无盘领域应用SSD时多用作读缓存来使用，SSD缓存源自  
    PXD无盘的SSD二级缓存技术。这里的二级缓存不是指CPU的二级CACHE，而是指应用于无  
    盘架构的第二级读缓存功能。 
  - <http://baike.baidu.com/view/3967982.htm>
  - <http://www.searchstorage.com.cn/showcontent_66616.htm>

* 快速块设备为较慢的块设备提供缓存
  - Red Hat Enterprise Linux 7.0 中引进让快速块设备作为较慢块设备的缓存的功能作为  
    技术预览。这个功能可让 PCIe SSD 设备作为直接附加存储（DAS）或者存储局域网（SAN）  
    存储的缓存使用，以便提高文件系统性能
  - [Reference](https://access.redhat.com/documentation/zh-CN/Red_Hat_Enterprise_Linux/7/html/7.0_Release_Notes/chap-storage.html)

#### Facebook flashcache
Flashcache是Facebook技术团队的一个开源项目，最初是为加速MySQL设计。Flashcache通过  
在文件系统（VFS）和设备驱动之间新增了一次缓存层，来实现对热门数据的缓存。

Flashcache在内核的层次：  
VFS -> Block层 -> DM层 -> flashcache -> DeviceDriver -> Disk

Flashcache最初的实现是write backup机制cache，后来又加入了write through和write around机制

	write backup: 先写入到cahce，然后cache中的脏块会由后台定期刷到持久存储。
	write through: 同步写入到cache和持久存储。
	write around: 只写入到持久存储。

参考:   
+ [Flashcache初探](http://blog.chinaunix.net/uid-26950862-id-3948198.html)  
+ [flashcache原理及改造（PPT）](http://oldblog.donghao.org/2012/12/flashcacheoaiaoippt.html)  
+ [flashcache原理及改造](http://wenku.baidu.com/view/4c37447131b765ce0508142b?refer=&clickSort=download)  

#### What skills need to master

#### Implementation

#### Reference
+ [linux内核源码阅读之facebook硬盘加速利器flashcache](http://blog.csdn.net/column/details/flashcache.html)
+ [发几个自己做的关于内核IO方面的ppt](http://bbs.chinaunix.net/forum.php?mod=viewthread&tid=1917560&page=1#pid13842158)
+ [flashcache](https://github.com/facebook/flashcache/)  
+ [Centos安装Flashcache使用SSD缓存](http://www.haiyun.me/archives/centos-flashcache.html)  
+ [flashcache的实现与分析](http://blog.csdn.net/kidd_3/article/details/6984822)  
+ [一种实现存储系统ssd缓存数据选择性升级的系统架构](https://awk.so/patents/CN103744624A?cl=zh)  
+ [命中率80%，磁盘I/O减半，Flashcache的发展史](http://www.csdn.net/article/2013-10-31/2817357-facebook-flashcache-2010-2013)  
+ [FlashCache at Facebook](http://cdn.oreillystatic.com/en/assets/1/event/45/Flashcache%20Presentation.pdf)  
+ [Flashcache初探](http://blog.chinaunix.net/uid-26950862-id-3948198.html)  

