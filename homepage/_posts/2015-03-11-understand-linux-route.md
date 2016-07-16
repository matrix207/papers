---
layout: post
title: "understand linux route"
description: ""
category: linux
tags: [linux]
---

### 路由规则理解
主机的网络封包需要会根据路由规则来判断如何将封包发送出去

路由是雙向的，你必須要瞭解出去的路由與回來時的規則

### 常用命令
* `route`  
  `route -n`  
  `route add default gw 172.16.130.1 eth2`  
  `route del default gw 172.16.130.1 eth2`  
  `route add -host 192.168.168.110 dev eth0`  
  `route del -host 192.168.168.110 dev eth0`  
  `route add -net 172.16.130.0/24 gw 172.16.130.1 eth2`  
  `route del -net 172.16.0.0 netmask 255.255.0.0 dev eth0`  
  `ip route flush cache`  
* `sysctl -a |grep forward`
* `sysctl -a |grep ignore`
* `echo 2 > /proc/sys/net/ipv4/conf/default/rp_filter`
* `echo 2 > /proc/sys/net/ipv4/conf/all/rp_filter`
* `sysctl -p`
* `vim /etc/sysctl.conf`
* `traceroute www.baidu.com`

### 相关配置文件

		[root@localhost ~]# cat /etc/iproute2/rt_tables
		#
		# reserved values
		#
		255	local
		254	main
		253	default
		0	unspec
		#
		# local
		#
		#1	inr.ruhep
		[root@localhost ~]# sysctl -a |grep ignore
		net.ipv4.conf.all.arp_ignore = 0
		net.ipv4.conf.default.arp_ignore = 0
		net.ipv4.conf.lo.arp_ignore = 0
		net.ipv4.conf.eth0.arp_ignore = 0
		net.ipv4.conf.eth1.arp_ignore = 0
		net.ipv4.icmp_echo_ignore_all = 0
		net.ipv4.icmp_echo_ignore_broadcasts = 1
		net.ipv4.icmp_ignore_bogus_error_responses = 1
		[root@localhost ~]# cat /etc/sysctl.conf 

### 例子

如果是双网卡且设定的是同一网段IP:   
* eth0 : 192.168.0.100
* eth1 : 192.168.0.200

那么一般会生成这样的路由规则:  

		[root@www ~]# route -n
		Kernel IP routing table
		Destination     Gateway   Genmask         Flags Metric Ref   Use Iface
		192.168.0.0     0.0.0.0   255.255.255.0   U     0      0       0 eth1
		192.168.0.0     0.0.0.0   255.255.255.0   U     0      0       0 eth0

也就是說:  
* 當要主動發送封包到192.168.0.0/24的網域時，都只會透過第一條規則，也就是透過eth1來傳出去!
* 在回應封包方面，不管是由eth0還是由eth1進來的網路封包，都會透過 eth1 來回傳!

来自: <http://linux.vbird.org/linux_server/0230router.php>, [8.1.3 重複路由的問題] 

### 只设置一个网口IP

		[dennis@localhost ~]$ ssh root@172.16.60.53
		root@172.16.60.53's password: 
		Last login: Wed Mar 11 17:54:12 2015 from 172.16.50.39
		[root@localhost ~]# route
		Kernel IP routing table
		Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
		172.16.60.0     *               255.255.255.0   U     0      0        0 eth2
		default         172.16.60.1     0.0.0.0         UG    0      0        0 eth2
		[root@localhost ~]# ip addr
		1: lo: <LOOPBACK,UP,LOWER_UP> mtu 16436 qdisc noqueue state UNKNOWN 
			link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
			inet 127.0.0.1/8 scope host lo
			inet6 ::1/128 scope host 
			   valid_lft forever preferred_lft forever
		2: eth0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc mq state DOWN qlen 1000
			link/ether a0:36:9f:32:b0:d0 brd ff:ff:ff:ff:ff:ff
		3: eth1: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc mq state DOWN qlen 1000
			link/ether a0:36:9f:32:b0:d1 brd ff:ff:ff:ff:ff:ff
		4: eth2: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP qlen 1000
			link/ether 00:25:90:f4:6b:60 brd ff:ff:ff:ff:ff:ff
			inet 172.16.60.53/24 brd 172.16.60.255 scope global eth2
			inet6 fe80::225:90ff:fef4:6b60/64 scope link 
			   valid_lft forever preferred_lft forever
		5: eth3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP qlen 1000
			link/ether 00:25:90:f4:6b:61 brd ff:ff:ff:ff:ff:ff
			inet6 fe80::225:90ff:fef4:6b61/64 scope link 
			   valid_lft forever preferred_lft forever

### 没有问题的设定

		[dennis@localhost ~]$ ssh root@172.16.130.105
		root@172.16.130.105's password: 
		Permission denied, please try again.
		root@172.16.130.105's password: 
		Last login: Wed Mar 11 16:34:38 2015 from 172.16.50.39
		[root@localhost ~]# route
		Kernel IP routing table
		Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
		172.16.130.0    *               255.255.255.0   U     0      0        0 eth0
		172.16.130.0    *               255.255.255.0   U     0      0        0 eth1
		default         172.16.130.1    0.0.0.0         UG    0      0        0 eth1
		default         172.16.130.1    0.0.0.0         UG    0      0        0 eth0
		[root@localhost ~]# ping 172.16.50.39 -I eth1
		PING 172.16.50.39 (172.16.50.39) from 172.16.130.106 eth1: 56(84) bytes of data.
		64 bytes from 172.16.50.39: icmp_seq=1 ttl=63 time=11.7 ms
		64 bytes from 172.16.50.39: icmp_seq=2 ttl=63 time=0.265 ms
		^C
		--- 172.16.50.39 ping statistics ---
		2 packets transmitted, 2 received, 0% packet loss, time 1568ms
		rtt min/avg/max/mdev = 0.265/6.030/11.795/5.765 ms
		[root@localhost ~]# ping 172.16.50.39 -I eth0
		PING 172.16.50.39 (172.16.50.39) from 172.16.130.105 eth0: 56(84) bytes of data.
		64 bytes from 172.16.50.39: icmp_seq=1 ttl=63 time=0.244 ms
		64 bytes from 172.16.50.39: icmp_seq=2 ttl=63 time=6.12 ms
		64 bytes from 172.16.50.39: icmp_seq=3 ttl=63 time=6.50 ms
		^C
		--- 172.16.50.39 ping statistics ---
		3 packets transmitted, 3 received, 0% packet loss, time 2414ms
		rtt min/avg/max/mdev = 0.244/4.290/6.504/2.866 ms
		[root@localhost ~]# ip addr
		1: lo: <LOOPBACK,UP,LOWER_UP> mtu 16436 qdisc noqueue state UNKNOWN 
			link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
			inet 127.0.0.1/8 scope host lo
			inet6 ::1/128 scope host 
			   valid_lft forever preferred_lft forever
		2: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP qlen 1000
			link/ether 00:25:90:de:fc:13 brd ff:ff:ff:ff:ff:ff
			inet 172.16.130.106/24 scope global eth1
			inet6 fe80::225:90ff:fede:fc13/64 scope link 
			   valid_lft forever preferred_lft forever
		3: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP qlen 1000
			link/ether 00:25:90:de:fc:12 brd ff:ff:ff:ff:ff:ff
			inet 172.16.130.105/24 scope global eth0
			inet6 fe80::225:90ff:fede:fc12/64 scope link 
			   valid_lft forever preferred_lft forever
		[root@localhost ~]# ip route show
		172.16.130.0/24 dev eth0  proto kernel  scope link  src 172.16.130.105 
		172.16.130.0/24 dev eth1  proto kernel  scope link  src 172.16.130.106 
		default via 172.16.130.1 dev eth1 
		default via 172.16.130.1 dev eth0 

### 一台奇怪问题的机器

问题机器: 双网卡，IP信息 eth0:172.16.60.150 eth1:172.16.60.151 网关172.16.60.1  

本地主机: IP 172.16.50.39, 网关:172.16.50.1

本地(50.39)可以正常ssh登陆60.150  

但ping 172.16.60.151没有回应:

		[root@localhost ~]# cat /etc/sysconfig/network-scripts/ifcfg-eth0
		DEVICE=eth0
		HWADDR=00:1e:67:c9:9a:f4
		TYPE=Ethernet
		UUID=5adb47dc-d361-43b7-a23a-f17c2ade1e2d
		ONBOOT=yes
		NM_CONTROLLED=yes
		BOOTPROTO=none
		IPADDR=172.16.60.150
		NETMASK=255.255.255.0
		GATEWAY=172.16.60.1
		IPV6INIT=no
		USERCTL=no
		[root@localhost ~]# cat /etc/sysconfig/network-scripts/ifcfg-eth1
		DEVICE=eth1
		HWADDR=00:1e:67:c9:9a:f5
		TYPE=Ethernet
		UUID=2af28c1e-abe2-4805-8c78-880ffbfc567e
		ONBOOT=yes
		NM_CONTROLLED=yes
		BOOTPROTO=none
		IPV6INIT=no
		USERCTL=no
		IPADDR=172.16.60.151
		NETMASK=255.255.255.0
		GATEWAY=172.16.60.1
		[root@localhost ~]# ip addr  
		1: lo: <LOOPBACK,UP,LOWER_UP> mtu 16436 qdisc noqueue state UNKNOWN   
		  link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00  
		  inet 127.0.0.1/8 scope host lo  
		  inet6 ::1/128 scope host   
			 valid_lft forever preferred_lft forever  
		2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP qlen 1000  
		  link/ether 00:1e:67:c9:9a:f4 brd ff:ff:ff:ff:ff:ff  
		  inet 172.16.60.150/24 brd 172.16.60.255 scope global eth0  
		  inet6 fe80::21e:67ff:fec9:9af4/64 scope link   
			 valid_lft forever preferred_lft forever  
		3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP qlen 1000  
		  link/ether 00:1e:67:c9:9a:f5 brd ff:ff:ff:ff:ff:ff  
		  inet 172.16.60.151/24 brd 172.16.60.255 scope global eth1  
		  inet6 fe80::21e:67ff:fec9:9af5/64 scope link   
			 valid_lft forever preferred_lft forever  
		[root@localhost ~]# route  
		Kernel IP routing table  
		Destination     Gateway         Genmask         Flags Metric Ref    Use Iface  
		172.16.60.0     *               255.255.255.0   U     0      0        0 eth0  
		172.16.60.0     *               255.255.255.0   U     0      0        0 eth1  
		link-local      *               255.255.0.0     U     1002   0        0 eth0  
		link-local      *               255.255.0.0     U     1003   0        0 eth1  
		default         172.16.60.1     0.0.0.0         UG    0      0        0 eth0  


通过同一网段的机器(172.16.60.53)，ping 151得到的是Destination Host Unreachable  

		[dennis@localhost ~]$ ssh root@172.16.60.53
		The authenticity of host '172.16.60.53 (172.16.60.53)' can't be established.
		RSA key fingerprint is e6:b0:c1:60:53:cd:77:7c:32:e4:27:4f:01:43:5a:8a.
		Are you sure you want to continue connecting (yes/no)? yes
		Warning: Permanently added '172.16.60.53' (RSA) to the list of known hosts.
		root@172.16.60.53's password: 
		Last login: Fri Dec  6 04:46:04 2013
		[root@localhost ~]# route
		Kernel IP routing table
		Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
		172.16.60.0     *               255.255.255.0   U     0      0        0 eth2
		default         172.16.60.1     0.0.0.0         UG    0      0        0 eth2
		[root@localhost ~]# ping 172.16.60.151
		PING 172.16.60.151 (172.16.60.151) 56(84) bytes of data.
		From 172.16.60.53 icmp_seq=2 Destination Host Unreachable
		From 172.16.60.53 icmp_seq=3 Destination Host Unreachable
		From 172.16.60.53 icmp_seq=4 Destination Host Unreachable
		^C
		--- 172.16.60.151 ping statistics ---
		5 packets transmitted, 0 received, +3 errors, 100% packet loss, time 4988ms
		pipe 3
		[root@localhost ~]# ping 172.16.60.150
		PING 172.16.60.150 (172.16.60.150) 56(84) bytes of data.
		64 bytes from 172.16.60.150: icmp_seq=1 ttl=64 time=0.933 ms
		64 bytes from 172.16.60.150: icmp_seq=2 ttl=64 time=0.235 ms
		64 bytes from 172.16.60.150: icmp_seq=3 ttl=64 time=0.223 ms
		^C
		--- 172.16.60.150 ping statistics ---
		3 packets transmitted, 3 received, 0% packet loss, time 2045ms
		rtt min/avg/max/mdev = 0.223/0.463/0.933/0.332 ms
		[root@localhost ~]# 

且从eth1(60.151)ping不通网关(172.16.60.1), ping同网段另外一台机器(60.53)开始会停顿:   

		[root@localhost ~]# arp
		Address                  HWtype  HWaddress           Flags Mask            Iface
		172.16.60.1              ether   00:0f:e2:b1:c7:5d   C                     eth0
		[root@localhost ~]# route
		Kernel IP routing table
		Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
		172.16.60.0     *               255.255.255.0   U     0      0        0 eth0
		172.16.60.0     *               255.255.255.0   U     0      0        0 eth1
		link-local      *               255.255.0.0     U     1002   0        0 eth0
		default         172.16.60.1     0.0.0.0         UG    0      0        0 eth0
		[root@localhost ~]# ping 172.16.60.53 -I eth1
		PING 172.16.60.53 (172.16.60.53) from 172.16.60.151 eth1: 56(84) bytes of data.
		64 bytes from 172.16.60.53: icmp_seq=10 ttl=64 time=1.08 ms
		64 bytes from 172.16.60.53: icmp_seq=11 ttl=64 time=0.228 ms
		64 bytes from 172.16.60.53: icmp_seq=12 ttl=64 time=0.230 ms
		64 bytes from 172.16.60.53: icmp_seq=13 ttl=64 time=0.226 ms
		64 bytes from 172.16.60.53: icmp_seq=14 ttl=64 time=0.238 ms
		64 bytes from 172.16.60.53: icmp_seq=15 ttl=64 time=0.225 ms
		64 bytes from 172.16.60.53: icmp_seq=16 ttl=64 time=0.227 ms
		^C
		--- 172.16.60.53 ping statistics ---
		16 packets transmitted, 7 received, 56% packet loss, time 15646ms
		rtt min/avg/max/mdev = 0.225/0.351/1.085/0.299 ms
		[root@localhost ~]# ping 172.16.50.39 -I eth1
		PING 172.16.50.39 (172.16.50.39) from 172.16.60.151 eth1: 56(84) bytes of data.
		From 172.16.60.151 icmp_seq=2 Destination Host Unreachable
		From 172.16.60.151 icmp_seq=3 Destination Host Unreachable
		From 172.16.60.151 icmp_seq=4 Destination Host Unreachable
		From 172.16.60.151 icmp_seq=6 Destination Host Unreachable
		From 172.16.60.151 icmp_seq=7 Destination Host Unreachable
		From 172.16.60.151 icmp_seq=8 Destination Host Unreachable
		^C
		--- 172.16.50.39 ping statistics ---
		8 packets transmitted, 0 received, +6 errors, 100% packet loss, time 7646ms
		pipe 3
		[root@localhost ~]# ping 172.16.60.1 -I eth1
		PING 172.16.60.1 (172.16.60.1) from 172.16.60.151 eth1: 56(84) bytes of data.

执行`route add default gw 172.16.60.1 eth1`后，可以ping 172.16.60.151,但是150有问题了.  

使用ssh root@172.16.60.151登陆看到的路由信息

		[root@localhost ~]# route
		Kernel IP routing table
		Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
		172.16.60.0     *               255.255.255.0   U     0      0        0 eth0
		172.16.60.0     *               255.255.255.0   U     0      0        0 eth1
		link-local      *               255.255.0.0     U     1002   0        0 eth0
		default         172.16.60.1     0.0.0.0         UG    0      0        0 eth1
		default         172.16.60.1     0.0.0.0         UG    0      0        0 eth0
		[root@localhost ~]# ping 172.16.60.1 -I eth0
		PING 172.16.60.1 (172.16.60.1) from 172.16.60.150 eth0: 56(84) bytes of data.
		64 bytes from 172.16.60.1: icmp_seq=1 ttl=255 time=8.88 ms
		64 bytes from 172.16.60.1: icmp_seq=2 ttl=255 time=6.85 ms
		64 bytes from 172.16.60.1: icmp_seq=3 ttl=255 time=1.74 ms
		^C
		--- 172.16.60.1 ping statistics ---
		3 packets transmitted, 3 received, 0% packet loss, time 2142ms
		rtt min/avg/max/mdev = 1.747/5.830/8.885/3.003 ms
		[root@localhost ~]# ping 172.16.60.1 -I eth1
		PING 172.16.60.1 (172.16.60.1) from 172.16.60.151 eth1: 56(84) bytes of data.
		^C
		--- 172.16.60.1 ping statistics ---
		17 packets transmitted, 0 received, 100% packet loss, time 16976ms

		[root@localhost ~]# ping 172.16.50.39 -I eth0
		PING 172.16.50.39 (172.16.50.39) from 172.16.60.150 eth0: 56(84) bytes of data.
		^C
		--- 172.16.50.39 ping statistics ---
		13 packets transmitted, 0 received, 100% packet loss, time 12830ms

		[root@localhost ~]# ping 172.16.50.39 -I eth1
		PING 172.16.50.39 (172.16.50.39) from 172.16.60.151 eth1: 56(84) bytes of data.
		64 bytes from 172.16.50.39: icmp_seq=1 ttl=63 time=0.245 ms
		64 bytes from 172.16.50.39: icmp_seq=2 ttl=63 time=0.296 ms
		^C
		--- 172.16.50.39 ping statistics ---
		2 packets transmitted, 2 received, 0% packet loss, time 1391ms
		rtt min/avg/max/mdev = 0.245/0.270/0.296/0.030 ms
		[root@localhost ~]# ping 172.16.60.53 -I eth0
		PING 172.16.60.53 (172.16.60.53) from 172.16.60.150 eth0: 56(84) bytes of data.
		64 bytes from 172.16.60.53: icmp_seq=1 ttl=64 time=0.252 ms
		64 bytes from 172.16.60.53: icmp_seq=2 ttl=64 time=0.196 ms
		^C
		--- 172.16.60.53 ping statistics ---
		2 packets transmitted, 2 received, 0% packet loss, time 1342ms
		rtt min/avg/max/mdev = 0.196/0.224/0.252/0.028 ms
		[root@localhost ~]# ping 172.16.60.53 -I eth1
		PING 172.16.60.53 (172.16.60.53) from 172.16.60.151 eth1: 56(84) bytes of data.
		64 bytes from 172.16.60.53: icmp_seq=1 ttl=64 time=0.236 ms
		64 bytes from 172.16.60.53: icmp_seq=2 ttl=64 time=0.232 ms
		64 bytes from 172.16.60.53: icmp_seq=3 ttl=64 time=0.240 ms
		^C
		--- 172.16.60.53 ping statistics ---
		3 packets transmitted, 3 received, 0% packet loss, time 2431ms
		rtt min/avg/max/mdev = 0.232/0.236/0.240/0.003 ms

**这时候应该怎么设置route呢?**

可是没有看出哪里有问题，发往172.16.60.XX ip段的都从eth0走，其他IP段的从eth0发送给默认网关172.16.60.1

### 参考
+ [Linux 網路偵錯](http://linux.vbird.org/linux_server/0150detect_network.php)
+ [Linux双网卡设置IP属于同一网段的问题](http://my.oschina.net/jing31/blog/6611)
+ [Linux系统IP路由的工作原理](http://os.51cto.com/art/201205/339800.htm)
+ [路由觀念與路由器設定](http://linux.vbird.org/linux_server/0230router.php)
+ [Linux策略路由](http://my.oschina.net/guol/blog/156607)
+ [linux下路由表详解](http://yuangeqingtian.blog.51cto.com/6994701/1218946)
+ [CentOS双网卡策略路由测试](http://www.centoscn.com/CentOS/Intermediate/2013/0915/1633.html)
+ [Linux双网卡(内外网)同时使用路由设置](http://my.oschina.net/7shell/blog/308887)
+ [linux双网卡实现局域网跨网段访问](http://forum.ubuntu.org.cn/viewtopic.php?f=54&t=327616)
+ [ping工作原理](http://blog.csdn.net/zhuying_linux/article/details/6770730)
+ [icmp request received, but doesn't reply](http://stackoverflow.com/questions/18536796/icmp-request-received-but-doesnt-reply)
+ <https://access.redhat.com/solutions/53031>
+ [杂谈icmp](http://fengyun.blog.51cto.com/532912/608713)
+ [LINUX ROUTE配置小记](http://leo0216.blog.51cto.com/188904/84863)
