---
layout: post
title: "NAS storage system performance optimization"
description: ""
category: storage
tags: [storage]
---
### NAS
Network attach storage, there are two protocol NFS(Linux) and CIFS(windows)

### NFS
1.use UDP or TCP  
2.modify rsize and wsize  
3.use sync or async  

### CIFS
1.smb or smb2  
2.modify rsize and wsize  
3.prefetch  

### Tools 
Watch the memory,disk I/O and net speed.     
1.vmstat  
2.cat /proc/cpuinfo, /proc/meminfo, /proc/net/dev  

### Reference
* [存储课堂:NAS存储系统性能优化攻略](http://www.huawei.com/ecommunity/bbs/10167439.html)
* [samba optimization and speed tuning for performance](https://calomel.org/samba_optimize.html)
* [make samba go faster](https://wiki.amahi.org/index.php/Make_Samba_Go_Faster)
* [samba性能调优](http://niyunjiu.iteye.com/blog/661141)
* [Mavericks NFS vs SMB Implementation - Speed Test](http://wisebyte.blogspot.com/2014/02/mavericks-nfs-vs-smb-implementation.html)
