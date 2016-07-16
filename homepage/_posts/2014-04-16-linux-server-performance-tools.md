---
layout: post
title: "linux server performance tools"
description: ""
category: program 
tags: [performance]
---

### web server performance test tools
* AB(Apache Bench) 

	ab -r -k -n 100000 -c 500 url

	-r 表示遇到错误继续
	-k 表示keepalive
	-n 表示总共请求的次数
	-c 表示每次请求的数量（即并发数）
	url即要请求的地址

以上命令表示请求总数达到10万后就停止

执行上面的命令前先使用ulimit -n 100000解除操作系统内的并发限制

	watch ab -r -k -n 100000 -c 500 url

watch命令会自动重复执行，每次执行时，自动间隔2秒

以上命令表示请求达到10万次后，自动休息2秒，然后重新请求


设置watch的间隔时间

	watch -d 3 ab -r -k -n 100000 -c 500 url

以上命令表示请求达到10万次后，自动休息3秒，然后重新请求 

* Nikto 

* piwik

### linux server performance monitors tools
* free -m 

* top , htop 

* ps 

* iostat 

* iftop 

### Reference
* [三大开源工具监控Apache Web服务器性能](http://os.51cto.com/art/201205/337302.htm)
* [Web服务器性能/压力测试工具http_load、webbench、ab、Siege使用教程](http://www.vpser.net/opt/webserver-test.html)
* [Linux服务器性能追踪以及服务器监控常用命令](http://www.drupal001.com/2012/07/linux-server-monitor/)
