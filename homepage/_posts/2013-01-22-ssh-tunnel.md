---
layout: post
title: "ssh tunel"
description: ""
category: tools
tags: [ssh]
---

引子:  
最近火车票抢票插件问题引起的连锁效应，导致github访问出现问题，不管是GFW墙了github  
还是github墙了China,总之结果是，无法更新github上的blog了(pull和push都失败).  

使用ssh隧道可以访问墙外的网站，那么是否使用同样(或类似的原理)来访问github的项目呢?  
为此，对ssh tunel进行了一番搜索研究，以下是一些收集的信息:  


+ `ssh -f -NC -D7070 username@sample.com`  浏览器翻墙用的命令  
   -f 后台运行  
   -N 仅作端口转发,不执行任何命令  
   -C 压缩数据  
   -D 指定动态端口  


+ [SSH Tunnelling (Port Forwarding)](http://www.rzg.mpg.de/networkservices/ssh-tunnelling-port-forwarding)

+ [隧道协议](http://zh.wikipedia.org/wiki/%E9%9A%A7%E9%81%93%E5%8D%8F%E8%AE%AE)

+ [隧道技术简介](http://blog.jianingy.com/2009/09/ssh%E9%9A%A7%E9%81%93%E6%8A%80%E6%9C%AF%E7%AE%80%E4%BB%8B/)

+ [SSH Tunnel using Putty](http://fvalinux.pixnet.net/blog/post/26923337-ssh-tunnel-using-putty)

+ [SSH command manual](http://www.freebsd.org/cgi/man.cgi?query=ssh)

TODO:  
如何通过ssh隧道来进行git操作(pull,push and so on)  
