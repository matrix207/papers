---
layout: post
title: "how to write a good web server"
description: ""
category: network
tags: [network]
---
### Server module
* 1.**select** based server  
	Advantage:  easy to implement  
	disadvantage: limit to fd, max is 1024 on most linux  
* 2.**/dev/poll** based server  
	Advantage:  
	disadvantage: same to select  
* 3.**epoll** based server  
	Advantage: fastly, not need to check each fd.  
	disadvantage: do more coding  
* 4.**thread** based server  
	Advantage: cheap to create, easy to share memory  
	disadvantage: instability, crash thread make entry process down because all
	              threads share the same addres space.  
* 5.**process** based server  
	Advantage: stable, crash process do not effect others.  
	disadvantage: create and kill processs overhead the web server.  
* 6.**hybrid process thread** server  
	Advantage:  
	disadvantage:  
* 7.**process-pool** and **thread-pool** based server  
    process pool, reference 
	[dynamic-pool-of-processes](http://stackoverflow.com/questions/16286208/dynamic-pool-of-processes)
	[Preforking: Process Pools](http://www.devshed.com/c/a/Administration/Design-and-Architecture/3/)
	[3](http://blog.chinaunix.net/uid-20481436-id-1941519.html)  
	Advantage:  
	disadvantage:  
* 8.**event** based server  
	Advantage:  
	disadvantage:  

### Biggest performance-killers
* 1.Data copies  
* 2.Context switches  
* 3.Memory allocation  
* 4.Lock contention  

### Reference 
* [**The C10K problem**](http://www.kegel.com/c10k.html#examples.nb.select)
* [**Scalable Network Programming**](http://bulk.fefe.de/scalable-networking.pdf)
* [High performance server architecture](http://pl.atyp.us/content/tech/servers.html)
* [Comparison of web server software](http://en.wikipedia.org/wiki/Comparison_of_lightweight_web_servers)
* [mongoose - easy to use web server](https://code.google.com/p/mongoose)
* [Architecture of a Linux based Web Server - Monkey HTTP server](http://edsiper.linuxchile.cl/blog/2013/02/27/architecture-of-a-linux-based-web-server/)
* [Monkey HTTP server](https://github.com/monkey/monkey)
* [A very simple HTTP server writen in c](http://blog.abhijeetr.com/2010/04/very-simple-http-server-writen-in-c.html)
* [HTTP-server](https://github.com/ashishc/Http-Server#!)
* [How do I write a web server in C on linux](http://stackoverflow.com/questions/2338775/how-do-i-write-a-web-server-in-c-on-linux)
* [webserver search on github](https://github.com/search?l=C&q=webserver&ref=cmdform&type=Repositories)
* [simple webserver for linux](http://jakash3.wordpress.com/2011/03/08/simple-webserver-for-linux/)
* [Web server software architectures](http://sce.umkc.edu/~leeyu/class/cs551-04/papers/noshaba.pdf)
* [chaos - c++ network event library](https://github.com/lyjdamzwf/chaos)
