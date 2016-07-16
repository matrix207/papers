---
layout: post
title: "connect wifi in linux terminal"
description: ""
category: network
tags: [network]
---

##1.工具介绍

### wireless-tools
   使用到的工具 iwlist、iwconfig、dhclient
   只适用于WEP加密方式, WPA方式需要使用wpa_supplicant
### wpa_supplicant
   可用于WPA加密方式

##2.编译源码
### wireless-tools
   1). 下载 wireless_tools.29.tar.gz  
     `wget http://www.hpl.hp.com/personal/Jean_Tourrilhes/Linux/wireless_tools.29.tar.gz`

   2). 编译、安装  
		`tar xvf wireless_tools.29.tar.gz`  
		`cd wireless_tools.29`  
		`make`  
		`make install`  

### wpa_supplicant
   1). 下载   
     `wget http://www.openssl.org/source/openssl-0.9.8e.tar.gz`  
     `wget http://hostap.epitest.fi/releases/wpa_supplicant-2.0.tar.gz`

   2). Reference:  
	 [http://blog.csdn.net/penglijiang/article/details/8573946](http://blog.csdn.net/penglijiang/article/details/8573946)  
	 [http://blog.csdn.net/ti_tantbx/article/details/7037741](http://blog.csdn.net/ti_tantbx/article/details/7037741)
 
##3.操作步骤
### wireless_tools
注意： 操作前提，无线路由器使用WEP方式加密

使用步骤:  

+ 使用命令ifconfig查找无线网卡接口，通常为wlan0  
+ 使用命令iwlist扫描无线网络  
+ 使用命令iwconfig设置要连接的无线网络  
+ 使用命令iwconfig设置无线网络密码  
+ 使用命令dhclient开始连接网络  
+ 使用命令ifconfig和ping验证是否无线网络设置成功  

下面是使用TP-Link无线网卡连接无线路由器的操作记录

    [root@localhost ~]# ifconfig
    [root@localhost ~]# iwlist wlan0 scanning
    [root@localhost ~]# iwconfig wlan0 essid TP_Link_Test
    [root@localhost ~]# iwconfig wlan0 key s:12349
    [root@localhost ~]# dhclient wlan0
    [root@localhost ~]# ifconfig wlan0
    wlan0     Link encap:Ethernet  HWaddr 14:CF:92:23:B5:BF  
              inet addr:192.168.1.101  Bcast:255.255.255.255  Mask:255.255.255.0
              inet6 addr: fe80::16cf:92ff:fe23:b5bf/64 Scope:Link
              UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
              RX packets:1141 errors:0 dropped:53252 overruns:0 frame:0
              TX packets:37 errors:0 dropped:0 overruns:0 carrier:0
              collisions:0 txqueuelen:1000 
              RX bytes:425292 (415.3 KiB)  TX bytes:6687 (6.5 KiB)

    说明：
    iwconfig wlan0 essid TP_Link_Test 这里的 TP_Link_Test 是无线网络名称
    iwconfig wlan0 key s:12349 这里的s:表示密码为ASCII格式

### wpa_supplicant
   
1). 生成配置文件  
    其中TP_Link_Test为无线网络名称, 1234567890为无线网络密码  
    `wpa_passphrase TP_Link_Test "1234567890" > /etc/wpa_supplicant.conf`

2). 修改配置/etc/wpa_supplicant/wpa_supplicant.conf  

		ctrl_interface=/var/run/wpa_supplicant
		ctrl_interface_group=wheel

		network={
			ssid="TP_Link_Test"
			proto=WPA
			key_mgmt=WPA-PSK
			pairwise=TKIP
			group=TKIP
			#psk="1234567890"
			psk=eed346e10ee03dd93b9d25abbc117d58be9f619cf7d6aa1c7f3c080bbe94b84e
		}

3). 连接网络  
    `wpa_supplicant -Dwext -iwlan0 -c /etc/wpa_supplicant/wpa_supplicant.conf`

	执行命令后可能出现"Operation not permitted",看似是错误信息，不用管。  
	[root@localhost ~]# wpa_supplicant -Dwext -iwlan0 -c /etc/wpa_supplicant/wpa_supplicant.conf 
	ioctl[SIOCSIWAP]: Operation not permitted
	Trying to associate with 1c:92:73:b0:02:33 (SSID='TP_Link_Test' freq=2412 MHz)
	Associated with 1c:92:73:b0:02:33
	WPA: Key negotiation completed with 1c:92:73:b0:02:33 [PTK=TKIP GTK=TKIP]
	CTRL-EVENT-CONNECTED - Connection to 1c:92:73:b0:02:33 completed (auth) [id=0 id_str=]
	WPA: Group rekeying completed with 1c:fa:68:b9:18:16 [GTK=TKIP]

4). 获取IP  
	`dhclient wlan0`  

5). 检查是否获取成功  
	`ifconfig wlan0`  
	ping 网关:  
	`ping 192.168.1.1`

6). 断开网络  
	`wpa_cli disable_network 0`  
	`wpa_cli remove_network 0`  
	`dhclient -x wlan0`  

Reference:  
+ [http://hostap.epitest.fi/wpa_supplicant/](http://hostap.epitest.fi/wpa_supplicant/)
+ [http://blog.163.com/wxiongn@126/blog/static/11788203820102262748358/] (http://blog.163.com/wxiongn@126/blog/static/11788203820102262748358/)
+ [http://xitong.iteye.com/blog/1723427] (http://xitong.iteye.com/blog/1723427)
+ [http://evan7s.blog.163.com/blog/static/108955356201132494921476/] (http://evan7s.blog.163.com/blog/static/108955356201132494921476/)
+ [http://blog.csdn.net/lin772662623/article/details/7830336] (http://blog.csdn.net/lin772662623/article/details/7830336)
+ [wpa_supplicant的移植和可能遇到的问题](http://blog.csdn.net/ti_tantbx/article/details/7037741)
+ [https://github.com/kimperator/Samen/blob/master/src/samen-handler/samen-handler.c](https://github.com/kimperator/Samen/blob/master/src/samen-handler/samen-handler.c)

##4.其他参考资料  
+ [Wireless Tools for Linux](http://www.hpl.hp.com/personal/Jean_Tourrilhes/Linux/Tools.html)
+ [http://blog.csdn.net/jacobywu/article/details/7366080](http://blog.csdn.net/jacobywu/article/details/7366080)
+ [Wi-Fi WLAN wireless home networking information](https://help.ubuntu.com/community/WifiDocs/WiFiHowTo)
+ [Fedora: Using the command line interface](docs.fedoraproject.org/en-US/Fedora/13/html/Wireless_Guide/sect-Wireless_Guide-Fedora_And_Wireless-iwconfig.html)
+ [getwifi, a bash script](http://sourceforge.net/projects/getwifi/?source=dlp)
