---
layout: post
title: "Message Queue Middleware"
description: ""
category: linux
tags: []
---

### Introduce
* MOM: Message Oriented Middleware
* Message Queue ( MQ ) is a middleware between applications and networks.

There are a number of open source choices of messaging middleware systems, including   
* JBoss Messaging 
* JORAM 
* [Apache ActiveMQ](http://en.wikipedia.org/wiki/Apache_ActiveMQ)
* Sun Open Message Queue 
* Apache Qpid
* [RabbitMQ](http://en.wikipedia.org/wiki/RabbitMQ)
* Beanstalkd 
* Tarantool 
* HTTPSQS
* [MSMQ](http://en.wikipedia.org/wiki/Microsoft_Message_Queuing)

### Compare
* Application A <--> Network <--> Application B
* Application A <--> MOM <---> Application B

### Standard and protocol
* AMQP

### Using ActiveMQ

**1.Download and run activemq**:  
* Download the latest activemq from <http://activemq.apache.org/download.html>
* tar xvf apache-activemq-5.11.0-bin.tar.gz 
* cd apache-activemq-5.11.0
* ./bin/activemq start 
* http://localhost:8161. The default username and password is admin/admin
* check running: 
  - netstat -apn |grep 61616
  - ps aux |grep activemq
* ./bin/activemq stop
* for more information, read docs/user-guide.html 

**2.Build openwire-cpp example**:  
* yum install activemq-cpp-devel
* cd /home/dennis/Downloads/apache-activemq-5.11.0/examples/openwire/cpp 
* gcc Listener.cpp -o listener -I/usr/include/activemq-cpp-3.8.3 -I/usr/include/apr-1 -lactivemq-cpp -lstdc++
* gcc Publisher.cpp -o publisher -I/usr/include/activemq-cpp-3.8.3 -I/usr/include/apr-1 -lactivemq-cpp -lstdc++

**3.Running example**:  

* 3.1 Run service as root 

	[dennis@localhost apache-activemq-5.11.0]$ su
	Password: 
	[root@localhost apache-activemq-5.11.0]# ./bin/activemq start
	INFO: Loading '/home/dennis/Downloads/apache-activemq-5.11.0/bin/env'
	INFO: Using java '/usr/bin/java'
	INFO: Process with pid '3510' is already running

* 3.2 Run listener  

	[dennis@localhost cpp]$ ./listener 
	=====================================================
	Starting the Listener example:
	-----------------------------------------------------
	Waiting for messages...
	Received 1000 messages.
	Received 2000 messages.
	Received 3000 messages.
	Received 4000 messages.
	Received 5000 messages.
	Received 6000 messages.
	Received 7000 messages.
	Received 8000 messages.
	Received 9000 messages.
	Received 10000 messages.
	Received 10000 in 1.606 seconds
	-----------------------------------------------------
	Finished with the example.
	=====================================================

* 3.3 Run publisher  

	[dennis@localhost cpp]$ ./publisher 
	=====================================================
	Starting the Publisher example:
	-----------------------------------------------------
	Sent 1000 messages
	Sent 2000 messages
	Sent 3000 messages
	Sent 4000 messages
	Sent 5000 messages
	Sent 6000 messages
	Sent 7000 messages
	Sent 8000 messages
	Sent 9000 messages
	Sent 10000 messages
	-----------------------------------------------------
	Finished with the example.
	=====================================================

### Reference
* [Message Queue](http://en.wikipedia.org/wiki/Message_queue)
* [Message-oriented middleware](http://en.wikipedia.org/wiki/Message-oriented_middleware)
* [消息队列中间件的技术选型分析](http://blog.fity.cn/post/377/)
* [Apache Kafka：下一代分布式消息系统](http://www.infoq.com/cn/articles/apache-kafka)
* ★★★[消息队列中间件调研文档](http://alibaba.github.io/RocketMQ-docs/document/openuser/mqvsmq.pdf)
* [消息中间件及WebSphere MQ入门](http://www.ibm.com/developerworks/cn/websphere/library/techarticles/loulijun/MQnewer/MQnewer.html)
* [The ZeroMQ project](https://github.com/zeromq)
* [api of zeroMQ](http://api.zeromq.org/)
