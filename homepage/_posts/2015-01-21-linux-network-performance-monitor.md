---
layout: post
title: "linux network performance monitor"
description: ""
category: tools
tags: [network, performance]
---

### ethtool

### iptraf 

### netperf

### iperf
Build from source:  

	[root@localhost ~]# tar xvf iperf-2.0.5.tar.gz 
	...
	[root@localhost ~]# cd iperf-2.0.5
	[root@localhost iperf-2.0.5]# ./configure 
	...
	[root@localhost iperf-2.0.5]# make
	...

On server machine:  

	[root@Ustor iperf-2.0.5]# ./src/iperf -s
	------------------------------------------------------------
	Server listening on TCP port 5001
	TCP window size: 85.3 KByte (default)
	------------------------------------------------------------
	[  4] local 192.16.110.80 port 5001 connected with 192.16.110.50 port 43341
	[ ID] Interval       Transfer     Bandwidth
	[  4]  0.0-10.0 sec  9.20 GBytes  7.90 Gbits/sec
	[  5] local 192.16.110.80 port 5001 connected with 192.16.110.50 port 43342
	[  5]  0.0-10.0 sec  9.26 GBytes  7.95 Gbits/sec
	[  4] local 192.16.110.80 port 5001 connected with 192.16.110.50 port 43343
	[  4]  0.0-10.0 sec  8.98 GBytes  7.71 Gbits/sec

On Client machine:  

	[root@localhost iperf-2.0.5]# ./src/iperf -c 192.16.110.80 -f M -i 2
	------------------------------------------------------------
	Client connecting to 192.16.110.80, TCP port 5001
	TCP window size: 0.02 MByte (default)
	------------------------------------------------------------
	[  3] local 192.16.110.50 port 43343 connected with 192.16.110.80 port 5001
	[ ID] Interval       Transfer     Bandwidth
	[  3]  0.0- 2.0 sec  1598 MBytes   799 MBytes/sec
	[  3]  2.0- 4.0 sec  1919 MBytes   960 MBytes/sec
	[  3]  4.0- 6.0 sec  1892 MBytes   946 MBytes/sec
	[  3]  6.0- 8.0 sec  1894 MBytes   947 MBytes/sec
	[  3]  8.0-10.0 sec  1893 MBytes   947 MBytes/sec
	[  3]  0.0-10.0 sec  9196 MBytes   920 MBytes/sec

* 通用参数
  - -f [k|m|K|M] 分别表示以Kbits, Mbits, KBytes, MBytes显示报告，默认以Mbits为单位,eg:iperf -c 222.35.11.23 -f K
  - -i sec 以秒为单位显示报告间隔，eg:iperf -c 222.35.11.23 -i 2
  - -l 缓冲区大小，默认是8KB,eg:iperf -c 222.35.11.23 -l 16
  - -m 显示tcp最大mtu值
  - -o 将报告和错误信息输出到文件eg:iperf -c 222.35.11.23 -o c:\iperflog.txt
  - -p 指定服务器端使用的端口或客户端所连接的端口eg:iperf -s -p 9999;iperf -c 222.35.11.23 -p 9999
  - -u 使用udp协议,测试htb的时候最好用udp，udp通信开销小，测试的带宽更准确
  - -w 指定TCP窗口大小，默认是8KB.如果窗口太小，有可能丢包
  - -B 绑定一个主机地址或接口（当主机有多个地址或接口时使用该参数）
  - -C 兼容旧版本（当server端和client端版本不一样时使用）
  - -M 设定TCP数据包的最大mtu值
  - -N 设定TCP不延时
  - -V 传输ipv6数据包
* server专用参数
  - -D 以服务方式运行ipserf，eg:iperf -s -D
  - -R 停止iperf服务，针对-D，eg:iperf -s -R
* client端专用参数
  - -d 同时进行双向传输测试
  - -n 指定传输的字节数，eg:iperf -c 222.35.11.23 -n 100000
  - -r 单独进行双向传输测试
  - -b 指定发送带宽，默认是1Mbit/s. 在测试qos的时候，这是最有用的参数。
  - -t 测试时间，默认10秒,eg:iperf -c 222.35.11.23 -t 5.默认是10s
  - -F 指定需要传输的文件
  - -T 指定ttl值 

### tcpdump tcptrace
* tcpdump -i eth0
* tcpdump -i eth0 host 172.16.30.44 and port 80
* tcpdump -U port 3260 -w /tmp/tcpdump.pcap

### Reference
* [Linux性能监测：network](http://www.vpsee.com/2009/11/linux-system-performance-monitoring-network/)
* [iperf 使用总结](http://blog.chinaunix.net/uid-20778443-id-94533.html)
