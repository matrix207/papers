---
layout: post
title: "how to detect the internal network topology of company"
description: ""
category: network
tags: [network]
---

### What I want
How can know how many route/hub/switch are in my LAN network?

### What I know
* Host LAN IP is : 172.16.50.39
* gateway is : 172.16.50.1
* WLan IP: 219.134.89.155

### Operations

		[dennis@localhost ~]$ ip addr
		1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default 
			link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
			inet 127.0.0.1/8 scope host lo
			   valid_lft forever preferred_lft forever
			inet6 ::1/128 scope host 
			   valid_lft forever preferred_lft forever
		2: enp2s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
			link/ether 00:23:ae:96:d6:9a brd ff:ff:ff:ff:ff:ff
			inet 172.16.50.39/24 brd 172.16.50.255 scope global enp2s0
			   valid_lft forever preferred_lft forever
			inet6 fe80::223:aeff:fe96:d69a/64 scope link 
			   valid_lft forever preferred_lft forever
		[dennis@localhost ~]$ route
		Kernel IP routing table
		Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
		^C
		[dennis@localhost ~]$ route -n
		Kernel IP routing table
		Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
		0.0.0.0         172.16.50.1     0.0.0.0         UG    1024   0        0 enp2s0
		172.16.50.0     0.0.0.0         255.255.255.0   U     0      0        0 enp2s0
		[dennis@localhost ~]$ curl ifconfig.me
		219.134.89.155
		[dennis@localhost ~]$ ping www.baidu.com
		PING www.a.shifen.com (180.97.33.108) 56(84) bytes of data.
		64 bytes from 180.97.33.108: icmp_seq=1 ttl=51 time=36.6 ms
		64 bytes from 180.97.33.108: icmp_seq=2 ttl=51 time=42.0 ms
		...
		^C
		--- www.a.shifen.com ping statistics ---
		120 packets transmitted, 120 received, 0% packet loss, time 138219ms
		rtt min/avg/max/mdev = 28.061/41.513/82.900/10.308 ms
		[dennis@localhost ~]$ traceroute www.baidu.com
		traceroute to www.baidu.com (180.97.33.107), 30 hops max, 60 byte packets
		 1  172.16.50.1 (172.16.50.1)  3.386 ms  3.431 ms  3.463 ms
		 2  219.134.89.153 (219.134.89.153)  6.355 ms  7.169 ms  7.155 ms
		 3  10.1.100.37 (10.1.100.37)  5.989 ms  6.274 ms  6.318 ms
		 4  121.15.130.18 (121.15.130.18)  7.180 ms  7.167 ms  7.153 ms
		 5  61.104.38.59.broad.fs.gd.dynamic.163data.com.cn (59.38.104.61)  6.227 ms  6.219 ms 57.104.38.59.broad.fs.gd.dynamic.163data.com.cn (59.38.104.57)  6.924 ms
		 6  183.56.65.30 (183.56.65.30)  31.614 ms 183.56.66.2 (183.56.66.2)  13.175 ms 183.56.65.38 (183.56.65.38)  12.152 ms
		 7  202.97.64.73 (202.97.64.73)  37.557 ms  37.543 ms  38.302 ms
		 8  202.102.69.114 (202.102.69.114)  42.036 ms  43.259 ms  43.246 ms
		 9  * * *
		10  180.97.32.2 (180.97.32.2)  37.346 ms 180.97.32.6 (180.97.32.6)  38.140 ms 180.97.32.22 (180.97.32.22)  36.722 ms
		11  * * *
		12  * * *
		13  * * *
		14  * * *
		15  * * *
		16  * * *
		17  * * *
		18  * * *
		19  * * *
		20  * * *
		21  * * *
		22  * * *
		23  * * *
		24  * * *
		25  * * *
		26  * * *
		27  * * *
		28  * * *
		29  * * *
		30  * * *
		[dennis@localhost ~]$ traceroute 219.134.89.155
		traceroute to 219.134.89.155 (219.134.89.155), 30 hops max, 60 byte packets
		 1  172.16.50.1 (172.16.50.1)  3.835 ms  3.856 ms  3.885 ms
		 2  * * *
		 3  * * *
		 4  * * *
		 5  * * *
		 6  * * *
		 7  * * *
		 8  * * *
		 9  * * *
		10  * * *
		11  * * *
		12  * * *
		13  * * *
		14  * * *
		15  * * *
		16  * * *
		17  * * *
		18  * * *
		19  * * *
		20  * * *
		21  * * *
		22  * * *
		23  * * *
		24  * * *
		25  * * *
		26  * * *
		27  * * *
		28  * * *
		29  * * *
		30  * * *
