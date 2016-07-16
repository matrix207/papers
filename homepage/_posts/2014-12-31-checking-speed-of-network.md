---
layout: post
title: "checking network speed"
description: ""
category: network 
tags: [network]
---

#### 一次网络性能检查记录

* 开始检查各种信息(网络、cpu,memory,io等)

		[root@Ustor ~]# ifconfig
		eth0      Link encap:Ethernet  HWaddr 40:16:7E:35:C7:C2  
				  inet addr:172.16.130.158  Bcast:0.0.0.0  Mask:255.255.255.0
				  inet6 addr: fe80::4216:7eff:fe35:c7c2/64 Scope:Link
				  UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
				  RX packets:37825154 errors:0 dropped:0 overruns:0 frame:0
				  TX packets:214565 errors:0 dropped:0 overruns:0 carrier:0
				  collisions:0 txqueuelen:1000 
				  RX bytes:2321429968 (2.1 GiB)  TX bytes:226462903 (215.9 MiB)
				  Interrupt:16 Memory:dc400000-dc420000 

		eth1      Link encap:Ethernet  HWaddr 40:16:7E:35:C7:C3  
				  UP BROADCAST MULTICAST  MTU:1500  Metric:1
				  RX packets:0 errors:0 dropped:0 overruns:0 frame:0
				  TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
				  collisions:0 txqueuelen:1000 
				  RX bytes:0 (0.0 b)  TX bytes:0 (0.0 b)
				  Interrupt:17 Memory:dc300000-dc320000 

		eth2      Link encap:Ethernet  HWaddr 00:90:FA:6C:E4:0A  
				  inet addr:192.16.110.50  Bcast:0.0.0.0  Mask:255.255.255.0
				  inet6 addr: fe80::290:faff:fe6c:e40a/64 Scope:Link
				  UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
				  RX packets:775026147 errors:0 dropped:1576 overruns:0 frame:0
				  TX packets:1032091357 errors:0 dropped:0 overruns:0 carrier:0
				  collisions:0 txqueuelen:1000 
				  RX bytes:892065492548 (830.8 GiB)  TX bytes:1034532529750 (963.4 GiB)

		eth3      Link encap:Ethernet  HWaddr 00:90:FA:6C:E4:0E  
				  inet6 addr: fe80::290:faff:fe6c:e40e/64 Scope:Link
				  UP BROADCAST MULTICAST  MTU:1500  Metric:1
				  RX packets:6593 errors:0 dropped:0 overruns:0 frame:0
				  TX packets:5748 errors:0 dropped:0 overruns:0 carrier:0
				  collisions:0 txqueuelen:1000 
				  RX bytes:2011976 (1.9 MiB)  TX bytes:345000 (336.9 KiB)

		lo        Link encap:Local Loopback  
				  inet addr:127.0.0.1  Mask:255.0.0.0
				  inet6 addr: ::1/128 Scope:Host
				  UP LOOPBACK RUNNING  MTU:16436  Metric:1
				  RX packets:21995 errors:0 dropped:0 overruns:0 frame:0
				  TX packets:21995 errors:0 dropped:0 overruns:0 carrier:0
				  collisions:0 txqueuelen:0 
				  RX bytes:1234012 (1.1 MiB)  TX bytes:1234012 (1.1 MiB)

		[root@Ustor ~]# ping 192.16.110.60
		PING 192.16.110.60 (192.16.110.60) 56(84) bytes of data.
		64 bytes from 192.16.110.60: icmp_seq=1 ttl=128 time=0.147 ms
		64 bytes from 192.16.110.60: icmp_seq=2 ttl=128 time=0.197 ms
		64 bytes from 192.16.110.60: icmp_seq=3 ttl=128 time=0.195 ms
		64 bytes from 192.16.110.60: icmp_seq=4 ttl=128 time=0.173 ms
		64 bytes from 192.16.110.60: icmp_seq=5 ttl=128 time=0.197 ms
		^C
		--- 192.16.110.60 ping statistics ---
		5 packets transmitted, 5 received, 0% packet loss, time 4079ms
		rtt min/avg/max/mdev = 0.147/0.181/0.197/0.026 ms

		[root@Ustor ~]# route 
		Kernel IP routing table
		Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
		172.16.130.0    *               255.255.255.0   U     0      0        0 eth0
		192.16.110.0    *               255.255.255.0   U     0      0        0 eth2
		default         192.16.110.1    0.0.0.0         UG    0      0        0 eth2
		default         172.16.130.1    0.0.0.0         UG    0      0        0 eth0
		[root@Ustor ~]# ethtool -i eth2
		driver: be2net
		version: 4.1.307r
		firmware-version: 10.0.803.19
		bus-info: 0000:02:00.0
		[root@Ustor ~]# ethtool eth2
		Settings for eth2:
			Supported ports: [ FIBRE ]
			Supported link modes:   10000baseT/Full 
			Supports auto-negotiation: No
			Advertised link modes:  Not reported
			Advertised pause frame use: No
			Advertised auto-negotiation: No
			Speed: 10000Mb/s
			Duplex: Full
			Port: FIBRE
			PHYAD: 0
			Transceiver: external
			Auto-negotiation: off
			Supports Wake-on: g
			Wake-on: d
			Link detected: yes
		[root@Ustor ~]# iostat 
		Linux 2.6.32-279.el6.x86_64 (Ustor) 	12/31/2014 	_x86_64_	(2 CPU)

		avg-cpu:  %user   %nice %system %iowait  %steal   %idle
				   0.36    0.00    1.23    1.08    0.00   97.33

		Device:            tps   Blk_read/s   Blk_wrtn/s   Blk_read   Blk_wrtn
		sda               3.56        45.00        24.38   19491848   10557699
		sdb              28.76      1274.39      2100.99  551950065  909958848
		dm-0            224.75       639.32      1158.70  276896736  501843480
		dm-1            197.05       634.96       941.44  275005928  407746632
		dm-2              0.01         0.02         0.85      10001     368272

		[root@Ustor ~]# fdisk -l

		Disk /dev/sda: 8012 MB, 8012390400 bytes
		255 heads, 63 sectors/track, 974 cylinders
		Units = cylinders of 16065 * 512 = 8225280 bytes
		Sector size (logical/physical): 512 bytes / 512 bytes
		I/O size (minimum/optimal): 512 bytes / 512 bytes
		Disk identifier: 0x53c628b9

		   Device Boot      Start         End      Blocks   Id  System
		/dev/sda1   *           1          26      204800   83  Linux
		Partition 1 does not end on cylinder boundary.
		/dev/sda2              26         287     2097152   82  Linux swap / Solaris
		Partition 2 does not end on cylinder boundary.
		/dev/sda3             287         300      102400   83  Linux
		Partition 3 does not end on cylinder boundary.
		/dev/sda4             300         975     5419224    5  Extended
		Partition 4 does not end on cylinder boundary.
		/dev/sda5             300         313      102400   83  Linux
		/dev/sda6             313         975     5314560   83  Linux

		Disk /dev/sdb: 36002.0 GB, 36002026487808 bytes
		255 heads, 63 sectors/track, 4376997 cylinders
		Units = cylinders of 16065 * 512 = 8225280 bytes
		Sector size (logical/physical): 512 bytes / 4096 bytes
		I/O size (minimum/optimal): 4096 bytes / 4096 bytes
		Disk identifier: 0x00000000


		Disk /dev/mapper/r55-i01: 2097.2 GB, 2097152000000 bytes
		255 heads, 63 sectors/track, 254964 cylinders
		Units = cylinders of 16065 * 512 = 8225280 bytes
		Sector size (logical/physical): 512 bytes / 4096 bytes
		I/O size (minimum/optimal): 4096 bytes / 4096 bytes
		Disk identifier: 0x2c0893c9


		Disk /dev/mapper/r55-i02: 2097.2 GB, 2097152000000 bytes
		255 heads, 63 sectors/track, 254964 cylinders
		Units = cylinders of 16065 * 512 = 8225280 bytes
		Sector size (logical/physical): 512 bytes / 4096 bytes
		I/O size (minimum/optimal): 4096 bytes / 4096 bytes
		Disk identifier: 0xcbbcf789


		Disk /dev/mapper/r55-ee: 349.5 GB, 349526032384 bytes
		255 heads, 63 sectors/track, 42494 cylinders
		Units = cylinders of 16065 * 512 = 8225280 bytes
		Sector size (logical/physical): 512 bytes / 4096 bytes
		I/O size (minimum/optimal): 4096 bytes / 4096 bytes
		Disk identifier: 0x00000000


		#### Hard Raid Card ####
		[root@Ustor ~]# lspci  
		00:00.0 Host bridge: Intel Corporation 2nd Generation Core Processor Family DRAM Controller (rev 09)
		00:01.0 PCI bridge: Intel Corporation Xeon E3-1200/2nd Generation Core Processor Family PCI Express Root Port (rev 09)
		00:1a.0 USB controller: Intel Corporation 6 Series/C200 Series Chipset Family USB Enhanced Host Controller #2 (rev 05)
		00:1c.0 PCI bridge: Intel Corporation 6 Series/C200 Series Chipset Family PCI Express Root Port 1 (rev b5)
		00:1c.4 PCI bridge: Intel Corporation 6 Series/C200 Series Chipset Family PCI Express Root Port 5 (rev b5)
		00:1c.5 PCI bridge: Intel Corporation 6 Series/C200 Series Chipset Family PCI Express Root Port 6 (rev b5)
		00:1d.0 USB controller: Intel Corporation 6 Series/C200 Series Chipset Family USB Enhanced Host Controller #1 (rev 05)
		00:1e.0 PCI bridge: Intel Corporation 82801 PCI Bridge (rev a5)
		00:1f.0 ISA bridge: Intel Corporation C202 Chipset Family LPC Controller (rev 05)
		00:1f.2 SATA controller: Intel Corporation 6 Series/C200 Series Chipset Family SATA AHCI Controller (rev 05)
		00:1f.3 SMBus: Intel Corporation 6 Series/C200 Series Chipset Family SMBus Controller (rev 05)
		01:00.0 RAID bus controller: LSI Logic / Symbios Logic MegaRAID SAS 2108 [Liberator] (rev 05)
		02:00.0 Ethernet controller: Emulex Corporation OneConnect 10Gb NIC (be3) (rev 01)
		02:00.1 Ethernet controller: Emulex Corporation OneConnect 10Gb NIC (be3) (rev 01)
		03:00.0 Ethernet controller: Intel Corporation 82574L Gigabit Network Connection
		04:00.0 Ethernet controller: Intel Corporation 82574L Gigabit Network Connection
		05:05.0 VGA compatible controller: ASPEED Technology, Inc. ASPEED Graphics Family (rev 10)

		[root@Ustor ~]# MegaCli -LdPdInfo -aAll
											 
		Adapter #0

		Number of Virtual Disks: 1
		Virtual Drive: 0 (Target Id: 0)
		Name                :r55
		RAID Level          : Primary-5, Secondary-0, RAID Level Qualifier-3
		Size                : 32.743 TB
		Parity Size         : 3.637 TB
		State               : Optimal
		Strip Size          : 64 KB
		Number Of Drives    : 10
		Span Depth          : 1
		Default Cache Policy: WriteBack, ReadAdaptive, Cached, Write Cache OK if Bad BBU
		Current Cache Policy: WriteBack, ReadAdaptive, Cached, Write Cache OK if Bad BBU
		Default Access Policy: Read/Write
		Current Access Policy: Read/Write
		Disk Cache Policy   : Enabled
		Encryption Type     : None
		Bad Blocks Exist: No
		Is VD Cached: No
		Number of Spans: 1
		Span: 0 - Number of PDs: 10

		[root@Ustor ~]# MegaCli -LdPdInfo -aAll |grep -i 'raw size'
		Raw Size: 3.638 TB [0x1d1c0beb0 Sectors]
		Raw Size: 3.638 TB [0x1d1c0beb0 Sectors]
		Raw Size: 3.638 TB [0x1d1c0beb0 Sectors]
		Raw Size: 3.638 TB [0x1d1c0beb0 Sectors]
		Raw Size: 3.638 TB [0x1d1c0beb0 Sectors]
		Raw Size: 3.638 TB [0x1d1c0beb0 Sectors]
		Raw Size: 3.638 TB [0x1d1c0beb0 Sectors]
		Raw Size: 3.638 TB [0x1d1c0beb0 Sectors]
		Raw Size: 3.638 TB [0x1d1c0beb0 Sectors]
		Raw Size: 3.638 TB [0x1d1c0beb0 Sectors]

		#### /dev/sdb的大小是36002GB, 即 36002/1024=35.158TB ####
		#### 可是整个raid的大小应该时3.637TB * 9 = 32.733TB才对, 为什么下面/dev/sdb是35.158TB呢 ####
		[root@Ustor ~]# fdisk -l /dev/sdb

		Disk /dev/sdb: 36002.0 GB, 36002026487808 bytes
		255 heads, 63 sectors/track, 4376997 cylinders
		Units = cylinders of 16065 * 512 = 8225280 bytes
		Sector size (logical/physical): 512 bytes / 4096 bytes
		I/O size (minimum/optimal): 4096 bytes / 4096 bytes
		Disk identifier: 0x00000000

		[root@Ustor ~]# iostat 1
		Linux 2.6.32-279.el6.x86_64 (Ustor) 	12/31/2014 	_x86_64_	(2 CPU)

		avg-cpu:  %user   %nice %system %iowait  %steal   %idle
				   0.37    0.00    1.25    1.08    0.00   97.30

		Device:            tps   Blk_read/s   Blk_wrtn/s   Blk_read   Blk_wrtn
		sda               3.57        46.38        24.39   20146336   10593379
		sdb              28.77      1270.72      2136.50  551950697  928014000
		dm-0            229.04       637.48      1194.85  276896840  518997016
		dm-1            196.74       633.13       940.80  275006032  408647752
		dm-2              0.01         0.02         0.85      10169     368768

		avg-cpu:  %user   %nice %system %iowait  %steal   %idle
				   0.00    0.00   18.59    0.00    0.00   81.41

		Device:            tps   Blk_read/s   Blk_wrtn/s   Blk_read   Blk_wrtn
		sda               0.00         0.00         0.00          0          0
		sdb              72.00         0.00     24584.00          0      24584
		dm-0              0.00         0.00         0.00          0          0
		dm-1           3072.00         0.00     24576.00          0      24576
		dm-2              1.00         0.00         8.00          0          8

		avg-cpu:  %user   %nice %system %iowait  %steal   %idle
				   0.00    0.00   11.50    0.00    0.00   88.50

		Device:            tps   Blk_read/s   Blk_wrtn/s   Blk_read   Blk_wrtn
		sda               0.00         0.00         0.00          0          0
		sdb              37.00         0.00     16400.00          0      16400
		dm-0              0.00         0.00         0.00          0          0
		dm-1           2048.00         0.00     16384.00          0      16384
		dm-2              2.00         0.00        16.00          0         16

		avg-cpu:  %user   %nice %system %iowait  %steal   %idle
				   5.05    0.00   18.69    0.00    0.00   76.26

		Device:            tps   Blk_read/s   Blk_wrtn/s   Blk_read   Blk_wrtn
		sda               0.00         0.00         0.00          0          0
		sdb              72.00         0.00     32768.00          0      32768
		dm-0              0.00         0.00         0.00          0          0
		dm-1           4096.00         0.00     32768.00          0      32768
		dm-2              0.00         0.00         0.00          0          0

		#### dd的速度达到1GB/s，说明瓶颈不在磁盘 ####
		[root@Ustor ~]# dd if=/dev/zero of=/dev/sdb bs=1M count=102400
		28309+0 records in
		28309+0 records out
		29684137984 bytes (30 GB) copied, 28.4613 s, 1.0 GB/s
		33270+0 records in
		33270+0 records out
		34886123520 bytes (35 GB) copied, 33.4982 s, 1.0 GB/s
		38234+0 records in
		38234+0 records out
		40091254784 bytes (40 GB) copied, 38.5252 s, 1.0 GB/s
		^C42879+0 records in
		42879+0 records out
		44961890304 bytes (45 GB) copied, 43.2484 s, 1.0 GB/s

		[root@Ustor ~]# ls /dev/r55/
		ee  i01  i02
		[root@Ustor ~]# ls /dev/r55/ee -l
		lrwxrwxrwx 1 root root 7 Dec 30 16:42 /dev/r55/ee -> ../dm-2
		[root@Ustor ~]# dd if=/dev/zero of=/dev/r55/ee bs=1M count=1024000
		2552+0 records in
		2552+0 records out
		2675965952 bytes (2.7 GB) copied, 2.31478 s, 1.2 GB/s
		7516+0 records in
		7516+0 records out
		7881097216 bytes (7.9 GB) copied, 7.34263 s, 1.1 GB/s

		[root@Ustor ~]# dd if=/dev/zero of=/dev/r55/i01 bs=1M count=1024000
		2869+0 records in
		2869+0 records out
		3008364544 bytes (3.0 GB) copied, 2.73699 s, 1.1 GB/s
		7841+0 records in
		7841+0 records out
		8221884416 bytes (8.2 GB) copied, 7.76994 s, 1.1 GB/s

		[root@Ustor ~]# dd if=/dev/zero of=/dev/r55/i02 bs=1M count=1024000
		^[OH2751+0 records in
		2751+0 records out
		2884632576 bytes (2.9 GB) copied, 2.53248 s, 1.1 GB/s
		7704+1 records in
		7704+0 records out
		8078229504 bytes (8.1 GB) copied, 7.56027 s, 1.1 GB/s

		[root@Ustor ~]# blockdev --report /dev/sdb
		RO    RA   SSZ   BSZ   StartSec            Size   Device
		rw   256   512  4096          0  36002026487808   /dev/sdb
		[root@Ustor ~]# blockdev --report /dev/r55/ee 
		RO    RA   SSZ   BSZ   StartSec            Size   Device
		rw 16384   512  4096          0    349526032384   /dev/r55/ee
		[root@Ustor ~]# blockdev --report /dev/r55/i01
		RO    RA   SSZ   BSZ   StartSec            Size   Device
		rw   256   512  4096          0   2097152000000   /dev/r55/i01
		[root@Ustor ~]# blockdev --report /dev/r55/i02
		RO    RA   SSZ   BSZ   StartSec            Size   Device
		rw   256   512  4096          0   2097152000000   /dev/r55/i0

		[root@Ustor ~]# sysctl -a |grep ipv4.tcp
		net.ipv4.tcp_timestamps = 1
		net.ipv4.tcp_window_scaling = 1
		net.ipv4.tcp_sack = 1
		net.ipv4.tcp_retrans_collapse = 1
		net.ipv4.tcp_syn_retries = 5
		net.ipv4.tcp_synack_retries = 5
		net.ipv4.tcp_max_orphans = 131072
		net.ipv4.tcp_max_tw_buckets = 131072
		net.ipv4.tcp_keepalive_time = 7200
		net.ipv4.tcp_keepalive_probes = 9
		net.ipv4.tcp_keepalive_intvl = 75
		net.ipv4.tcp_retries1 = 3
		net.ipv4.tcp_retries2 = 15
		net.ipv4.tcp_fin_timeout = 60
		net.ipv4.tcp_syncookies = 1
		net.ipv4.tcp_tw_recycle = 0
		net.ipv4.tcp_abort_on_overflow = 0
		net.ipv4.tcp_stdurg = 0
		net.ipv4.tcp_rfc1337 = 0
		net.ipv4.tcp_max_syn_backlog = 1024
		net.ipv4.tcp_orphan_retries = 0
		net.ipv4.tcp_fack = 1
		net.ipv4.tcp_reordering = 3
		net.ipv4.tcp_ecn = 2
		net.ipv4.tcp_dsack = 1
		net.ipv4.tcp_mem = 173856	231808	347712
		net.ipv4.tcp_wmem = 4096	16384	4194304
		net.ipv4.tcp_rmem = 4096	87380	4194304
		net.ipv4.tcp_app_win = 31
		net.ipv4.tcp_adv_win_scale = 2
		net.ipv4.tcp_tw_reuse = 0
		net.ipv4.tcp_frto = 2
		net.ipv4.tcp_frto_response = 0
		net.ipv4.tcp_low_latency = 0
		net.ipv4.tcp_no_metrics_save = 0
		net.ipv4.tcp_moderate_rcvbuf = 1
		net.ipv4.tcp_tso_win_divisor = 3
		net.ipv4.tcp_congestion_control = cubic
		net.ipv4.tcp_abc = 0
		net.ipv4.tcp_mtu_probing = 0
		net.ipv4.tcp_base_mss = 512
		net.ipv4.tcp_workaround_signed_windows = 0
		net.ipv4.tcp_dma_copybreak = 4096
		net.ipv4.tcp_slow_start_after_idle = 1
		net.ipv4.tcp_available_congestion_control = cubic reno
		net.ipv4.tcp_allowed_congestion_control = cubic reno
		net.ipv4.tcp_max_ssthresh = 0
		net.ipv4.tcp_thin_linear_timeouts = 0
		net.ipv4.tcp_thin_dupack = 0

		[root@Ustor ~]# ethtool -k eth2
		Offload parameters for eth2:
		rx-checksumming: on
		tx-checksumming: on
		scatter-gather: on
		tcp-segmentation-offload: on
		udp-fragmentation-offload: off
		generic-segmentation-offload: on
		generic-receive-offload: off
		large-receive-offload: off

		[root@Ustor ~]# tcpdump -i eth2 -w /tmp/tcpdump01.pcap
		tcpdump: listening on eth2, link-type EN10MB (Ethernet), capture size 65535 bytes
		^C154277 packets captured
		979189 packets received by filter
		824910 packets dropped by kernel
		[root@Ustor ~]# ls /tmp/tcpdump01.pcap -lh
		-rw-r--r-- 1 root root 187M Dec 31 10:47 /tmp/tcpdump01.pcap


* tshark 分析

		#### scp下载包到本地，使用tshark分析 ####
		[dennis@localhost ~]$ scp root@172.16.130.158:/tmp/tcpdump01.pcap ./
		root@172.16.130.158's password: 
		tcpdump01.pcap                         100%  186MB  11.0MB/s   00:17 
		[dennis@localhost ~]$ capinfos tcpdump01.pcap 
		File name:           tcpdump01.pcap
		File type:           Wireshark/tcpdump/... - pcap
		File encapsulation:  Ethernet
		Packet size limit:   file hdr: 65535 bytes
		Number of packets:   154 k
		File size:           195 MB
		Data size:           192 MB
		Capture duration:    40 seconds
		Start time:          Wed Dec 31 10:47:13 2014
		End time:            Wed Dec 31 10:47:53 2014
		Data byte rate:      4,816 kBps
		Data bit rate:       38 Mbps
		Average packet size: 1249.82 bytes
		Average packet rate: 3,853 packets/sec
		SHA1:                b08cff272eb5b0d9aa64fb31343314603df44309
		RIPEMD160:           70123da08082954af0d91dde9d9766a61c0a93db
		MD5:                 0571ac4f35410047a13cdfbd3bf3bfe9
		Strict time order:   False

		#### 重传好像影响不大? ####
		[dennis@localhost ~]$ tshark -n -q -r tcpdump01.pcap  -z "io,stat,0,tcp.analysis.retransmission"

		=======================================================
		| IO Statistics                                       |
		|                                                     |
		| Interval size: 40.0 secs (dur)                      |
		| Col 1: Frames and bytes                             |
		|     2: tcp.analysis.retransmission                  |
		|-----------------------------------------------------|
		|              |1                   |2                |
		| Interval     | Frames |   Bytes   | Frames |  Bytes |
		|-----------------------------------------------------|
		|  0.0 <> 40.0 | 154277 | 192818724 |    141 | 191110 |
		=======================================================
		[dennis@localhost ~]$ tshark -n -q -r tcpdump01.pcap  -z "io,stat,0,tcp.analysis.out_of_order"

		=======================================================
		| IO Statistics                                       |
		|                                                     |
		| Interval size: 40.0 secs (dur)                      |
		| Col 1: Frames and bytes                             |
		|     2: tcp.analysis.out_of_order                    |
		|-----------------------------------------------------|
		|              |1                   |2                |
		| Interval     | Frames |   Bytes   | Frames |  Bytes |
		|-----------------------------------------------------|
		|  0.0 <> 40.0 | 154277 | 192818724 |    620 | 898920 |
		=======================================================
		[dennis@localhost ~]$ tshark -n -q -r tcpdump01.pcap  -z "io,stat,5,tcp.analysis.out_of_order"

		==================================================
		| IO Statistics                                  |
		|                                                |
		| Interval size: 5 secs                          |
		| Col 1: Frames and bytes                        |
		|     2: tcp.analysis.out_of_order               |
		|------------------------------------------------|
		|          |1                  |2                |
		| Interval | Frames |   Bytes  | Frames |  Bytes |
		|------------------------------------------------|
		|  0 <>  5 |  10837 | 14246599 |      1 |   1514 |
		|  5 <> 10 |  49803 | 61236860 |    184 | 264780 |
		| 10 <> 15 |  48890 | 61739426 |    186 | 271384 |
		| 15 <> 20 |  42976 | 55481613 |    249 | 361242 |
		| 20 <> 25 |    397 |    24870 |      0 |      0 |
		| 25 <> 30 |    461 |    29156 |      0 |      0 |
		| 30 <> 35 |    488 |    31931 |      0 |      0 |
		| 35 <> 40 |    421 |    28029 |      0 |      0 |
		| 40 <> 40 |      4 |      240 |      0 |      0 |
		==================================================
		[dennis@localhost ~]$ tshark -n -q -r tcpdump01.pcap  -z "io,stat,5,tcp.analysis.retransmission"

		=================================================
		| IO Statistics                                 |
		|                                               |
		| Interval size: 5 secs                         |
		| Col 1: Frames and bytes                       |
		|     2: tcp.analysis.retransmission            |
		|-----------------------------------------------|
		|          |1                  |2               |
		| Interval | Frames |   Bytes  | Frames | Bytes |
		|-----------------------------------------------|
		|  0 <>  5 |  10837 | 14246599 |      3 |  4542 |
		|  5 <> 10 |  49803 | 61236860 |     62 | 85484 |
		| 10 <> 15 |  48890 | 61739426 |     41 | 53682 |
		| 15 <> 20 |  42976 | 55481613 |     35 | 47402 |
		| 20 <> 25 |    397 |    24870 |      0 |     0 |
		| 25 <> 30 |    461 |    29156 |      0 |     0 |
		| 30 <> 35 |    488 |    31931 |      0 |     0 |
		| 35 <> 40 |    421 |    28029 |      0 |     0 |
		| 40 <> 40 |      4 |      240 |      0 |     0 |
		=================================================
		[dennis@localhost ~]$ 


* 会不会网卡驱动有问题?据说这个Emulex卡直接装就可以用，不用单独安装驱动

		[root@Ustor ~]# modinfo be2net
		filename:       /lib/modules/2.6.32-279.el6.x86_64/kernel/drivers/net/benet/be2net.ko
		license:        GPL
		author:         ServerEngines Corporation
		description:    ServerEngines BladeEngine 10Gbps NIC Driver 4.1.307r
		version:        4.1.307r
		srcversion:     7076CA6C80C3CD968BAFFFC
		alias:          pci:v000010DFd00000720sv*sd*bc*sc*i*
		alias:          pci:v000010DFd0000E228sv*sd*bc*sc*i*
		alias:          pci:v000010DFd0000E220sv*sd*bc*sc*i*
		alias:          pci:v000019A2d00000710sv*sd*bc*sc*i*
		alias:          pci:v000019A2d00000700sv*sd*bc*sc*i*
		alias:          pci:v000019A2d00000221sv*sd*bc*sc*i*
		alias:          pci:v000019A2d00000211sv*sd*bc*sc*i*
		depends:        
		vermagic:       2.6.32-279.el6.x86_64 SMP mod_unload modversions 
		parm:           num_vfs:Number of PCI VFs to initialize (uint)
		parm:           multi_rxq:Obsolete and used only for compatibility (bool)
		parm:           rx_frag_size:Size of a fragment that holds rcvd data. (ushort)
		[root@Ustor ~]# lspci -nn |grep Emulex
		02:00.0 Ethernet controller [0200]: Emulex Corporation OneConnect 10Gb NIC (be3) [19a2:0710] (rev 01)
		02:00.1 Ethernet controller [0200]: Emulex Corporation OneConnect 10Gb NIC (be3) [19a2:0710] (rev 01)
		型号是19a2:0710
		[root@Ustor ~]# grep -irn '19a2.*0710' /lib/modules/2.6.32-279.el6.x86_64/modules.alias
		3732:alias pci:v000019A2d00000710sv*sd*bc*sc*i* be2net

* 修改磁盘预读大小，性能没什么提升，依然再100MB/s左右，查看windows下的网络曲线，不够平整

		[root@Ustor SOURCES]# blockdev --report
		RO    RA   SSZ   BSZ   StartSec            Size   Device
		rw 16384   512  4096          0      8012390400   /dev/sda
		rw 16384   512  1024       2048       209715200   /dev/sda1
		rw 16384   512  4096     411648      2147483648   /dev/sda2
		rw 16384   512  1024    4605952       104857600   /dev/sda3
		rw 16384   512  1024    4810752            1024   /dev/sda4
		rw 16384   512  1024    4812800       104857600   /dev/sda5
		rw 16384   512  4096    5019648      5442109440   /dev/sda6
		rw   256   512  4096          0  36002026487808   /dev/sdb
		rw   256   512  4096          0   2097152000000   /dev/dm-0
		rw   256   512  4096          0   2097152000000   /dev/dm-1
		rw 16384   512  4096          0    349526032384   /dev/dm-2
		[root@Ustor SOURCES]# blockdev --setra 16384 /dev/dm-1
		[root@Ustor SOURCES]# blockdev --setra 16384 /dev/dm-0
		[root@Ustor SOURCES]# blockdev --setra 16384 /dev/sdb
		[root@Ustor SOURCES]# blockdev --report
		RO    RA   SSZ   BSZ   StartSec            Size   Device
		rw 16384   512  4096          0      8012390400   /dev/sda
		rw 16384   512  1024       2048       209715200   /dev/sda1
		rw 16384   512  4096     411648      2147483648   /dev/sda2
		rw 16384   512  1024    4605952       104857600   /dev/sda3
		rw 16384   512  1024    4810752            1024   /dev/sda4
		rw 16384   512  1024    4812800       104857600   /dev/sda5
		rw 16384   512  4096    5019648      5442109440   /dev/sda6
		rw 16384   512  4096          0  36002026487808   /dev/sdb
		rw 16384   512  4096          0   2097152000000   /dev/dm-0
		rw 16384   512  4096          0   2097152000000   /dev/dm-1
		rw 16384   512  4096          0    349526032384   /dev/dm-2

发现4k;0% read;0% random的速度比1M;0%read;0%random的快，
通常都是1M的快。。。
4k有10G的25%左右速度，速度曲线相对平整；而1M只有8%左右，而且速度曲线极其不平整；


* 使用rmp source编译驱动

		rpm -ivh be2net-10.2.470.14-1.src.rpm
		cd rpmbuild/SOURCES
		tar xvf be2net-10.2.470.14.tar.gz 
		cd be2net-10.2.470.14 
		make
		cp /lib/modules/2.6.32-279.el6.x86_64/kernel/drivers/net/benet/be2net.ko{,.bak}
		cp ./be2net.ko /lib/modules/2.6.32-279.el6.x86_64/kernel/drivers/net/benet/be2net.ko
		modinfo be2net
		reboot
		ethtool -i be2net

		[root@Ustor be2net-10.2.470.14]# modinfo ./be2net.ko
		filename:       ./be2net.ko
		supported:      external
		license:        GPL
		author:         Emulex Corporation
		description:    Emulex OneConnect NIC Driver 10.2.470.14
		version:        10.2.470.14
		srcversion:     DE6CC9D92A3CE9C042DA005
		alias:          pci:v000010DFd00000728sv*sd*bc*sc*i*
		alias:          pci:v000010DFd00000730sv*sd*bc*sc*i*
		alias:          pci:v000010DFd00000720sv*sd*bc*sc*i*
		alias:          pci:v000010DFd0000E228sv*sd*bc*sc*i*
		alias:          pci:v000010DFd0000E220sv*sd*bc*sc*i*
		alias:          pci:v000019A2d00000710sv*sd*bc*sc*i*
		alias:          pci:v000019A2d00000700sv*sd*bc*sc*i*
		alias:          pci:v000019A2d00000221sv*sd*bc*sc*i*
		alias:          pci:v000019A2d00000211sv*sd*bc*sc*i*
		depends:        
		vermagic:       2.6.32-279.el6.x86_64 SMP mod_unload modversions 
		parm:           rss_on_mc:Enable RSS in multi-channel functions with the capability. Disabled by default. (ushort)
		parm:           tx_prio:Create priority based TX queues. Disabled by default (uint)
		parm:           num_vfs:Number of PCI VFs to initialize (uint)
		parm:           rx_frag_size:Size of receive fragment buffer - 2048 (default), 4096 or 8192 (ushort)
		parm:           gro:Enable or Disable GRO. Enabled by default (uint)
		parm:           emi_canceller:Enable or Disable EMI Canceller. Disabled by default (uint)

* 更新驱动，重启机器

		[root@Ustor be2net-10.2.470.14]# cp be2net.ko /lib/modules/2.6.32-279.el6.x86_64/kernel/drivers/net/benet/
		[root@Ustor be2net-10.2.470.14]# ls /lib/modules/2.6.32-279.el6.x86_64/kernel/drivers/net/benet/
		be2net.ko  be2net.ko.bak
		[root@Ustor be2net-10.2.470.14]# modinfo be2net
		filename:       /lib/modules/2.6.32-279.el6.x86_64/kernel/drivers/net/benet/be2net.ko
		supported:      external
		license:        GPL
		author:         Emulex Corporation
		description:    Emulex OneConnect NIC Driver 10.2.470.14
		version:        10.2.470.14
		srcversion:     DE6CC9D92A3CE9C042DA005
		alias:          pci:v000010DFd00000728sv*sd*bc*sc*i*
		alias:          pci:v000010DFd00000730sv*sd*bc*sc*i*
		alias:          pci:v000010DFd00000720sv*sd*bc*sc*i*
		alias:          pci:v000010DFd0000E228sv*sd*bc*sc*i*
		alias:          pci:v000010DFd0000E220sv*sd*bc*sc*i*
		alias:          pci:v000019A2d00000710sv*sd*bc*sc*i*
		alias:          pci:v000019A2d00000700sv*sd*bc*sc*i*
		alias:          pci:v000019A2d00000221sv*sd*bc*sc*i*
		alias:          pci:v000019A2d00000211sv*sd*bc*sc*i*
		depends:        
		vermagic:       2.6.32-279.el6.x86_64 SMP mod_unload modversions 
		parm:           rss_on_mc:Enable RSS in multi-channel functions with the capability. Disabled by default. (ushort)
		parm:           tx_prio:Create priority based TX queues. Disabled by default (uint)
		parm:           num_vfs:Number of PCI VFs to initialize (uint)
		parm:           rx_frag_size:Size of receive fragment buffer - 2048 (default), 4096 or 8192 (ushort)
		parm:           gro:Enable or Disable GRO. Enabled by default (uint)
		parm:           emi_canceller:Enable or Disable EMI Canceller. Disabled by default (uint)
		[root@Ustor be2net-10.2.470.14]# modprobe be2net
		[root@Ustor be2net-10.2.470.14]# ethtool eth2
		Settings for eth2:
			Supported ports: [ FIBRE ]
			Supported link modes:   10000baseT/Full 
			Supports auto-negotiation: No
			Advertised link modes:  Not reported
			Advertised pause frame use: No
			Advertised auto-negotiation: No
			Speed: 10000Mb/s
			Duplex: Full
			Port: FIBRE
			PHYAD: 0
			Transceiver: external
			Auto-negotiation: off
			Supports Wake-on: g
			Wake-on: d
			Link detected: yes
		[root@Ustor be2net-10.2.470.14]# ethtool -i eth2
		driver: be2net
		version: 4.1.307r
		firmware-version: 10.0.803.19
		bus-info: 0000:02:00.0
		[root@Ustor be2net-10.2.470.14]# reboot

		Broadcast message from root@Ustor
			(/dev/pts/1) at 15:50 ...
		[dennis@localhost ~]$ ssh root@172.16.130.158
		root@172.16.130.158's password: 
		Last login: Wed Dec 31 09:32:07 2014 from 172.16.50.39
		[root@Ustor ~]# 
		[root@Ustor ~]# ethtool -i eth2
		driver: be2net
		version: 10.2.470.14
		firmware-version: 10.0.803.19
		bus-info: 0000:02:00.0
		[root@Ustor ~]# 

重新使用iometer测试看看，是否换了驱动性能有所提升  
10G网络使用率为65%，速度达到780MB/s左右, 网络速度曲线还算平整  

把驱动还原为旧的版本，在测试一下，如果速度又是100MB/s左右，说明确实是驱动问题。  
 
经过测试，速度确实又回到100MB/s左右了，看来是驱动问题了。  

#### 关于Emulex万兆网卡测试，性能比较低(只有约100MB/s)
1. 硬RAID，10块磁盘建raid5，建iscsi卷
2. windows使用iometer，1M;0%Read;0%Random
3. 网卡驱动be2net从4.1.307r升级到10.2.470.14

经检查测试，更新存储机器(172.16.130.158)的驱动版本(网卡驱动be2net从 4.1.307r升级到10.2.470.14) 后

使用iometer(1M;0%Read;0%Random)测试，速度从100MB/s左右提升到780MB/s左右

看起来原系统的Emulex万兆网卡驱动有问题，升级就好了.

驱动下载地址:

* <http://www.emulex.cn/downloads/emulex/drivers/linux/rhel-6-centos-6/drivers/>

* `wget http://www-dl.emulex.com/support/elx/rt10.2.9/ga/Linux/driver-source/be2net-10.2.470.14-1.src.rpm`


#### 总结: 很多时候，复杂问题的背后隐藏的是简单的解决方法
