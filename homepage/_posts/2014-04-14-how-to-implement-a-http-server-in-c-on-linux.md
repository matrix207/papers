---
layout: post
title: "how to implement a http server in c on linux"
description: ""
category: network
tags: [network]
---

### 1.use existing open source
* Apache 
* Nginx

### 2.use librarys
* Python HTTPServer / CGIHTTPServer
* libevent

### 3.implement it in c
* basic (learn from APUE/UNP)  
1.HTTP protocol(RFC 2616)  
2.TCP server(select/poll/epoll)  
3.multi-process model(fork, one proces per connection)  
4.multi-thread model(one thread per connection)  
5.IPC (interprocess communication)  
6.NON-Block I/O, edge trigger  

* advanced (learn from open source, lots pagers)  
1.performance optimists: memory cached, I/O(linux AIO), TCP/IP  
2.support CGI/FastCGI/WSGI/AJK  
3.support HTTPS  
4.dynamic modules  
5.support cluster  

### 4.Reference
* [1][实现一个http服务器需要怎样进行？需要哪些知识呢](http://www.zhihu.com/question/20199473)
* [2][how-do-i-write-a-web-server-in-c-on-linux](http://stackoverflow.com/questions/2338775/how-do-i-write-a-web-server-in-c-on-linux)
* [3][Comparison of lightweight web servers](http://en.wikipedia.org/wiki/Comparison_of_lightweight_web_servers)
* [4][libevent webserver in 40 lines of c](http://www.ruilog.com/article/view/5249.html)
