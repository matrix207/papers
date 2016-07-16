---
layout: post
title: "how to fix NIC name confusion"
description: ""
category: network 
tags: [linux, network]
---

#### Description

list some text from command `dmesg`:  

	792 igb 0000:02:00.0: Intel(R) Gigabit Ethernet Network Connection
	793 igb 0000:02:00.0: eth0: (PCIe:2.5GT/s:Width x1)
	794 igb 0000:02:00.0: eth0: MAC: 00:1e:67:58:cc:b8
	795 igb 0000:02:00.0: eth0: PBA No: 009000-000
	796 igb 0000:02:00.0: LRO is disabled
	797 igb 0000:02:00.0: Using MSI-X interrupts. 1 rx queue(s), 1 tx queue(s)
	798 igb 0000:03:00.0: PCI INT A -> GSI 18 (level, low) -> IRQ 18
	799 igb 0000:03:00.0: setting latency timer to 64
	800   alloc irq_desc for 32 on node -1
	801   alloc kstat_irqs on node -1
	802 igb 0000:03:00.0: irq 32 for MSI/MSI-X
	803   alloc irq_desc for 33 on node -1
	804   alloc kstat_irqs on node -1
	805 igb 0000:03:00.0: irq 33 for MSI/MSI-X
	806 igb 0000:03:00.0: Intel(R) Gigabit Ethernet Network Connection
	807 igb 0000:03:00.0: eth1: (PCIe:2.5GT/s:Width x1)
	808 igb 0000:03:00.0: eth1: MAC: 00:1e:67:58:cc:b9
	809 igb 0000:03:00.0: eth1: PBA No: 009000-000
	810 igb 0000:03:00.0: LRO is disabled
	811 igb 0000:03:00.0: Using MSI-X interrupts. 1 rx queue(s), 1 tx queue(s)
	812 udev: renamed network interface eth0 to eth2
	813 udev: renamed network interface eth1 to eth3

we can see that, interface name is eth0 and eth1 at the begin, then udev rename them.  

why?

#### Checking and fix

check the rule of udev:

	[root@Ustor network]# cat /etc/udev/rules.d/70-persistent-net.rules
	SUBSYSTEM=="net", ACTION=="add", DRIVERS=="?*", ATTR{address}=="00:25:90:d7:be:b4", ATTR{type}=="1", KERNEL=="eth*", NAME="eth0"
	SUBSYSTEM=="net", ACTION=="add", DRIVERS=="?*", ATTR{address}=="00:25:90:d7:be:b5", ATTR{type}=="1", KERNEL=="eth*", NAME="eth1"

check ip information:  

	[root@Ustor network]# ip addr
	1: lo: <LOOPBACK,UP,LOWER_UP> mtu 16436 qdisc noqueue state UNKNOWN 
		link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
		inet 127.0.0.1/8 scope host lo
		inet6 ::1/128 scope host 
		   valid_lft forever preferred_lft forever
	2: eth2: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP qlen 1000
		link/ether 00:1e:67:58:cc:b8 brd ff:ff:ff:ff:ff:ff
		inet 172.16.60.220/24 brd 172.16.60.255 scope global eth2
		inet6 fe80::21e:67ff:fe58:ccb8/64 scope link 
		   valid_lft forever preferred_lft forever
	3: eth3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP qlen 1000
		link/ether 00:1e:67:58:cc:b9 brd ff:ff:ff:ff:ff:ff
		inet6 fe80::21e:67ff:fe58:ccb9/64 scope link 
		   valid_lft forever preferred_lft forever

we found that, the mac address from udev rules and `ip addr` were difference.  

Because udev had defined which NIC used eth0 and eth1, the new NIC(new mac address)  
should used new interface name(here, using eth2 and eth3).

BTW, I ask the workmate what he had done before this problem occur, he said this  
os was installed on another board before. That was the problem, the NIC changed,   
but os not update, so it maded the interface name changed.

Fixed:  
1. update mac address to /etc/udev/rules.d/70-persistent-net.rules.mb2  
2. reboot  

#### Reference

