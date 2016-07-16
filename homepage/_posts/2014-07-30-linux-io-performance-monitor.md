---
layout: post
title: "Linux IO performance monitor"
description: ""
category: storage
tags: [performance]
---

### What your should know

* Your system parameters:
	- storage device: HDD, SSD (which?), Raid (which?)
	- filesystem, block size, journal mode
	- file cache, dirty thresholds, amount of memory
	- IO scheduler, its tunables
	- number of CPUs
	- kernel version

* Your test parameters:

	- read or write performance?
	- sequential or random?
	- 1 thread or multiple?
	- size of requests
	- optimize for throughput or request delay?

* IO’s Per Second（IOPS） 

	- IOPS=1000/(tseek+trotation+transfer), tseek:3~15ms, trotation:2ms, Transfer<<1  
      IOPS=1000/(5+2)=142

* random io and sequential io

### Monitor
* start from dd test (install centos 7 to u-disk)

	[root@localhost Downloads]# dd if=/home/dennis/Downloads/CentOS-7.0-1406-x86_64-DVD.iso of=/dev/sdc

* using /proc/diskstats

		[dennis@localhost Downloads]$ cat /proc/diskstats
		   8   0 sda 141736 59689 8916560 2493999 102005 133358 13766420 6019722 1 954049 8513774
		   8   1 sda1 286 562 3822 5720 7 1 28 156 0 4959 5875
		   8   2 sda2 141324 59017 8910850 2482584 90409 133357 13766392 5797210 1 762103 8279724
		 253   0 dm-0 14247 0 113976 102438 61899 0 495192 6397303 0 45071 6499746
		 253   1 dm-1 151287 0 5630082 3598603 47156 0 1021320 5247386 0 551034 8846229
		 253   2 dm-2 35848 0 3166248 352901 115404 0 12249880 2422762 1 524204 2775689
		   8  16 sdb 152 871 2311 604 0 0 0 0 0 564 604
		   8  17 sdb1 70 801 1095 280 0 0 0 0 0 262 280
		   8  32 sdc 248265 108 1986984 316895 5710 170143 1370400 48417446 158 346876 49258416
		   8  33 sdc1 248249 108 1986856 316866 5710 170143 1370400 48417446 158 346847 49258387


		- [dennis@localhost Downloads]$ cat /proc/diskstats | grep "sdc "
		  下面的显示去掉了 "8   32 sd "
		  +   1     2    3        4    5     6        7      8       9     10      11
		  + 132725 108 1062664 167643 2681  81983  643440  24022041  147 177399  24801880
		  + 274044 108 2193216 348847 6582  195199 1579680 54459810  150 388700  55296467
		  + 480307 108 3843320 608357 13408 392863 3217920 101917275 139 719423  103013695
		  + 636845 108 5095624 802159 18568 542271 4456320 138060107 132 971278  139338459 
		  + 717802 108 5743280 902925 21254 620223 5100960 156763894 134 1101859 158182895 
		- 第1个域：读完成次数 ----- 读磁盘的次数，成功完成读的总次数。
		- 第2个域：合并读完成次数， 第6个域：合并写完成次数。为了效率可能会合并相邻
				   的读和写。从而两次4K的读在它最终被处理到磁盘上之前可能会变成一次
				   8K的读，才被计数（和排队），因此只有一次I/O操作。这个域使你知
		  道这样的操作有多频繁。
		- 第3个域：读扇区的次数，成功读过的扇区总次数。
		- 第4个域：读花费的毫秒数，这是所有读操作所花费的毫秒数
				   (用make_request()到end_that_request_last()测量)
		- 第5个域：写完成次数 ----写完成的次数，成功写完成的总次数。
		- 第6个域：合并写完成次数 -----合并写次数。
		- 第7个域：写扇区次数 ---- 写扇区的次数，成功写扇区总次数。
		- 第8个域：写操作花费的毫秒数  ---  写花费的毫秒数，这是所有写操作所花费的毫秒数
		- 第9个域：正在处理的输入/输出请求数 -- -I/O的当前进度，只有这个域应该是0。
				   当请求被交给适当的request_queue_t时增加和请求完成时减小。
		- 第10个域：输入/输出操作花费的毫秒数  ----花在I/O操作上的毫秒数，这个域会增长只要field 9不为0。
		- 第11个域：输入/输出操作花费的加权毫秒数 -----  加权，花在I/O操作上的毫秒数，
					在每次I/O开始，I/O结束，I/O合并时这个域都会增加。这可以给I/O完成
					时间和存储那些可以累积的提供一个便利的测量标准。

* using vmstat

		[dennis@localhost Downloads]$ vmstat 1 10
		procs -----------memory---------- ---swap-- -----io---- -system-- ----cpu----
		 r  b   swpd   free   buff  cache   si   so   bi    bo   in   cs us sy id wa
		 0  2 262060  79832 1290864 736688  0    2    82    80  165  143  5  1 92  2
		 0  2 262060  81964 1279336 746696  0    0  5176  3840 5693 10504  5 10 23 62
		 0  2 262060  75888 1282288 749680  0    0  5896  3840 5259 11483  5  5 41 49
		 0  2 262060  70184 1285204 752636  0    0  5860     0 5213 11354  6  2 39 53
		 1  2 262060  79092 1281472 747172  0    0  5668  3904 5047 11017  5  4 43 49
		 1  2 262060  79584 1284256 743532  0    0  5832     0 5004 11202  5  2 43 50
		 0  2 262060  77532 1287020 742796  0    0  5672  3840 5126 11111  6  3 39 53
		 0  2 262060  79668 1289484 738468  0    0  5316     0 5598 10422  5  5 23 68
		 0  2 262060  79536 1290632 737000  0    0  5180  3840 4860 10482  6  3 41 50
		 0  2 262060  79880 1291936 735188  0    0  5160     0 5469 10400  5  6 27 62

		[dennis@localhost Downloads]$ vmstat -d
		disk- ------------reads------------ ------------writes----------- -----IO------
			   total merged sectors      ms  total merged sectors      ms   cur   sec
		sda   188079  70330 16034138 2811643 108057 169484 14406740 8013520   0  1118
		dm-0   15970      0  127760  112213  93653      0  749224  16348510   0    70
		dm-1  181140      0 6578234 4012264  51721      0 1236960   7818610   0   681
		dm-2   61516      0 9321616  477028 121227      0 12420520  2505467   0   622
		sdb      152    871    2311     604      0      0       0         0   0     0
		sdc   1012855   108 8103704 1284706  33810 978925 8101888 241555468   0  1636

* using top(using `man top` for help)   

* using iostat(using `man iostat` for help)   

		[dennis@localhost Downloads]$ su -c 'yum install sysstat -y'
		[dennis@localhost Downloads]$ iostat -x 1 3
		Linux 3.15.6-200.fc20.x86_64 (localhost.localdomain) 	07/30/2014 	_x86_64_	(2 CPU)

		avg-cpu:  %user   %nice %system %iowait  %steal   %idle
				   4.59    0.15    1.48    2.20    0.00   91.59

		Device:         rrqm/s   wrqm/s     r/s     w/s    rkB/s    wkB/s avgrq-sz avgqu-sz   await r_await w_await  svctm  %util
		sda               0.46     1.11    1.25    0.72    53.26    48.59   103.44     0.07   36.97   15.02   75.16   3.76   0.74
		dm-0              0.00     0.00    0.10    0.61     0.42     2.43     8.00     0.11  156.53    7.06  182.19   0.65   0.05
		dm-1              0.00     0.00    1.21    0.34    21.67     4.18    33.36     0.08   51.41   22.36  152.90   2.90   0.45
		dm-2              0.00     0.00    0.41    0.80    31.16    41.98   120.98     0.02   16.56    7.58   21.12   3.43   0.41
		sdb               0.01     0.00    0.00    0.00     0.01     0.00    15.20     0.00    3.97    3.97    0.00   3.71   0.00
		sdc               0.00     6.08    6.77    0.21    27.07    25.02    14.94     1.52  217.29    1.27 7226.26   1.52   1.06

		avg-cpu:  %user   %nice %system %iowait  %steal   %idle
				   2.91    0.00    9.22   52.91    0.00   34.95

		Device:         rrqm/s   wrqm/s     r/s     w/s    rkB/s    wkB/s avgrq-sz avgqu-sz   await r_await w_await  svctm  %util
		sda               0.00   660.00   22.00    8.00  2816.00  3404.00   414.67     0.08    2.83    0.50    9.25   1.63   4.90
		dm-0              0.00     0.00    0.00  666.00     0.00  2664.00     8.00     7.13   10.84    0.00   10.84   0.06   3.80
		dm-1              0.00     0.00    0.00    0.00     0.00     0.00     0.00     0.00    0.00    0.00    0.00   0.00   0.00
		dm-2              0.00     0.00   22.00    0.00  2816.00     0.00   256.00     0.01    0.50    0.50    0.00   0.50   1.10
		sdb               0.00     0.00    0.00    0.00     0.00     0.00     0.00     0.00    0.00    0.00    0.00   0.00   0.00
		sdc               0.00     0.00  719.00   15.00  2876.00  1800.00    12.74   150.38  197.33    1.32 9592.53   1.36 100.00

		avg-cpu:  %user   %nice %system %iowait  %steal   %idle
				   3.06    0.00    3.57   51.02    0.00   42.35

		Device:         rrqm/s   wrqm/s     r/s     w/s    rkB/s    wkB/s avgrq-sz avgqu-sz   await r_await w_await  svctm  %util
		sda               0.00     0.00   23.00    1.00  2944.00     4.00   245.67     0.01    0.50    0.52    0.00   0.50   1.20
		dm-0              0.00     0.00    0.00    0.00     0.00     0.00     0.00     0.00    0.00    0.00    0.00   0.00   0.00
		dm-1              0.00     0.00    0.00    1.00     0.00     4.00     8.00     0.00    0.00    0.00    0.00   0.00   0.00
		dm-2              0.00     0.00   23.00    0.00  2944.00     0.00   256.00     0.01    0.52    0.52    0.00   0.52   1.20
		sdb               0.00     0.00    0.00    0.00     0.00     0.00     0.00     0.00    0.00    0.00    0.00   0.00   0.00
		sdc               0.00   928.00  733.00   15.00  2932.00  1800.00    12.65   136.75  211.08    1.36 10459.40   1.34 100.00
		[dennis@localhost Downloads]$ watch iostat -x 

* using shell script monitor_io_stats.sh

		#!/bin/sh
		/etc/init.d/syslog stop
		echo 1 > /proc/sys/vm/block_dump
		sleep 60
		dmesg | awk '/(READ|WRITE|dirtied)/ {process[$1]++} END {for (x in process) \
		print process[x],x}' |sort -nr |awk '{print $2 " " $1}' | \
		head -n 10
		echo 0 > /proc/sys/vm/block_dump
		/etc/init.d/syslog start

### Other tools
* IOMeter, using for testing block device
  - Test parameters
    + of Outstanding I/Os, using `8` per target
    + each worker select one target?
    + 100% Write or 100% Read
    + Sequential or Random
    + Transfer Request Size using 1MB
    + Result Since, select `Last Update`
    + Update Frequency(seconds) select `1`

* SANergy, using for testing filesystem(Samba)
  - Select `Performance Tester`
  - Select driver(mapping the director as a driver, such as Z)
  - Select `Read test` or `Write test`
  - File size(MB) using `10240`
  - Record size(KB) using `1024`
  - select 'Loop'
  - start test

* Iozone: a filesystem benchmark tool
  - download from <http://www.iozone.org/src/current/>
  - compile source code: `make linux-AMD64`(NOTICE: check you cpu model and select correct command for compile)
  - test for nfs
    + showmount -e 172.16.130.111
    + mkdir /mnt/nastest
    + mount -t nfs 172.16.130.111:/share/nfs1 /mnt/nastest
    + ./iozone -az -b /mnt/nastest/test.xls -g 256G -y 32k -i 0 -i 1   

* FIO  
  - read: `fio --filename=/dev/sda3 --direct=1 --iodepth 1 --thread --rw=read --ioengine=psync --bs=4k --size=2G --numjobs=10 --runtime=100 --group_reporting --name=mytest`
  - write: `fio --filename=/dev/sda3 --direct=1 --iodepth 1 --thread --rw=write --ioengine=psync --bs=4k --size=2G --numjobs=10 --runtime=100 --group_reporting --name=mytest`
  - random read: `fio --filename=/dev/sda3 --direct=1 --iodepth 1 --thread --rw=randread --ioengine=psync --bs=4k --size=2G --numjobs=10 --runtime=100 --group_reporting --name=mytest`
  - random write: `fio --filename=/dev/sda3 --direct=1 --iodepth 1 --thread --rw=randwrite --ioengine=psync --bs=4k --size=2G --numjobs=10 --runtime=100 --group_reporting --name=mytest`
  - Mix random read and write: `fio --filename=/dev/sda3 --direct=1 --iodepth 1 --thread --rw=randrw --rwmixread=70 --ioengine=psync --bs=4k --size=2G --numjobs=10 --runtime=100 --group_reporting --name=mytest --ioscheduler=noop`

* blktrace: is a block layer IO tracing mechanism which provides detailed information  
            about request queue operations up to user space.
  - read: `fio --filename=/dev/sda3 --direct=1 --iodepth 1 --thread --rw=read --ioengine=psync --bs=4k --size=2G --numjobs=10 --runtime=100 --group_reporting --name=mytest`

### Example
* Troubleshooting high `uitl` of `iostat` command
  - using `iostat -x 2 5` to view the disk information

* Troubleshooting high `uitl` of `iostat` command

### Reference
* [Linux性能监测：磁盘IO篇](http://os.51cto.com/art/201012/239886.htm)
* [Linux Performance Monitoring with Vmstat and Iostat Commands](http://www.tecmint.com/linux-performance-monitoring-with-vmstat-and-iostat-commands/)
* [查看linux服务器硬盘IO读写负载](http://www.cnblogs.com/mfryf/archive/2012/03/12/2392012.html)
* [/proc/diskstats](http://blog.csdn.net/tenfyguo/article/details/7477526)
* [Testing IO performance in Linux](http://stackoverflow.com/a/3549620)
* [linux 查看磁盘IO状态操作指南](http://www.jb51.net/LINUXjishu/65741.html)
* [random and sequential io](http://bannaych.blogspot.jp/2005/12/random-and-sequential-io.html)
* [hdd seek time](http://en.wikipedia.org/wiki/Hard_disk_drive#Seek_time)
* [impact of sequential and random io](https://www.usenix.org/legacy/events/fast04/tech/full_papers/radkov/radkov_html/node14.html)
* [linux下的CPU、内存、IO、网络的压力测试](http://wushank.blog.51cto.com/3489095/1585927)
* [磁盘IOPS计算与测量](http://blog.csdn.net/liuaigui/article/details/6168186)
* <http://dadaru.blog.51cto.com/218979/481394>
* [系统性能分析工具&&一些我对磁盘IOPS的简单认识](http://dadaru.blog.51cto.com/218979/481394)
* [IO系统性能之一：衡量性能的几个指标](http://www.dbabeta.com/2009/io-performence-01_several-concepts.html)
* [fio性能测试工具新添图形前端gfio](http://blog.yufeng.info/archives/tag/fio)
* [linux 使用FIO测试磁盘iops](http://blog.itpub.net/26855487/viewspace-754346/)
* ★★★[Troubleshooting High I/O Wait in Linux](http://bencane.com/2012/08/06/troubleshooting-high-io-wait-in-linux/)
* ★★★[通过blktrace, debugfs分析磁盘IO](http://blogread.cn/it/article/6145)

