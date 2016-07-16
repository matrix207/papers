---
layout: post
title: "NetworkStack important structure"
description: ""
category: network 
tags: [kernel,network]
---

Version: linux-kernel-1.2.13

	File                Struct                        Description
	-------------------------------------------------------------------
	path: /include/linux
	etherdevice.h                                     以太网协议相关函数声明
	icmp.h                                            ICMP协议结构定议
	                    icmphdr
                        icmp_err
	if.h                                              接口相关结构定议
	                    ifmap
                        ifreq
                        ifconf
	if_arp.h                                          ARP协议结构定议
	                    arpreq
                        arphdr
	if_ether.h                                        以太网首部及标志位定义
	                    ethhdr
                        enet_statistics
	if_plip.h                                         并行线网络协议
	                    plip_conf
	if_slip.h                                         串行线协议
	igmp.h                                            IGMP协议结构定义
	                    igmphdr
                        ip_mc_socklist
                        ip_mc_list
	in.h                                              协议号定义
	                    in_addr
                        ip_mreq
                        sockaddr_in
	inet.h                                            INET域部分函数声明
	interrupt.h                                       下半部分结构定义
	                    bh_struct
	ip.h                                              IP协议结构定义
	                    timestamp
                        route
                        iphdr
	ip_fw.h                                           防火墙相关结构定义
	                    ip_fw
                        ip_fwpkt
	ipx.h                                             IPX包交换协议结构定义
	                    sockaddr_ipx
                        ipx_route_definition
                        ipx_interface_definition
                        ipx_config_data
                        ipx_route_def
	net.h                                             INET层关键结构定义
	                    socket
                        proto_ops
                        net_proto
	netdevice.h                                       设备相关结构定义
	                    de_mc_list
                        device
                        packet_type
	notifier.h                                        事件响应相关结构定义
	                    notifier_block
	ppp.h                                             点对点协议结构定义
	                    ppp_lqp_packet_hdr
                        ppp_lqp_packet_trailer
                        ppp_lqp_packet
                        ppp_ddinfo
                        ppp
	route.h                                           路由结构定义
	                    old_rtentry
                        rtentry
	skbuff.h                                          数据包封装结构定义
	                    sk_buff_head
                        sk_buff
	socket.h                                          常数选项定义
	                    sockaddr
                        linger
	sockios.h                                         选项定义
	tcp.h                                             TCP协议结构定义
	                    tcphdr
	timer.h                                           定时器相关结构定义
	                    timer_struct
                        timer_list
	udp.h                                             UDP协议结构定义
	                    udphdr
	un.h                                              UNIX域地址结构定义
	                    sockaddr_un

	path: net/inet
	ip.h
	                    ipfraq
                        ipq 
	ipx.h
	                    ipx_address
                        ipx_packet
                        ipx_interface
                        ipx_route
	protocol.h
	                    inet_protocol
	route.h
	                    rtable
	snmp.h
	                    ip_mib
                        icmp_mib
                        tcp_mib
                        udp_mib
	sock.h
	                    sock
                        proto

	path: include/linux
	fs.h
	                    file
	                    inode
	                    file_operations
	                    inode_operations

-----

Structure and memberships

	+---------------------------+              +------------------------+
	|struct sk_buff             |              |struct sk_buff_head     |
	+---------------------------+              +------------------------+
	|struct sk_buff	*next;      |              |struct sk_buff  *next;  |
	|struct sk_buff	*prev;      |              |struct sk_buff  *prev;  |
	|struct sk_buff	*link3;     |              +------------------------+
	|struct sk_buff *mem_addr;  |              
	|struct sock *sk;           |              +--------------------------+ 
	|struct device *dev;        |              |struct proto              |
	|... ...                    |              +--------------------------+
	+---------------------------+              |struct sock *sock_array[];|
                                               |int (*init) ();           |
                                               |int (*setsockopt) ();     |
	+----------------------------+             |int (*read) ();           |
	|struct device               |             |int (*write) ();          |
	+----------------------------+             |int (*connect) ();        |
	|struct device *next;        |             |int (*sendto) ();         |
	|struct device *slave;       |             |int (*recvfrom)();        |
	|struct sk_buff_head buffs[];|             |... ...                   |
	|... ...                     |             +--------------------------+
	+----------------------------+             

	+----------------------------------+       +----------------------------------+
	|struct socket                     |       |struct sock                       |
	+----------------------------------+       +----------------------------------+
	|short                type;        |       |struct sock *next;                |
	|socket_state         state;       |       |struct sk_buff_head write_queue;  |
	|long                 flags;       |       |struct sk_buff_head receive_queue;|
	|struct proto_ops     *ops;        |       |struct proto *prot;               |
	|void                 *data;       |       |struct socket *socket;            |
	|struct socket        *conn;       |       |struct sk_buff *send_head;        |
	|struct socket        *iconn;      |       |struct sk_buff *send_tail;        |
	|struct socket        *next;       |       |struct sk_buff_head back_log;     |
	|struct wait_queue    **wait;      |       |struct sk_buff *partial;          |
	|struct inode         *inode;      |       |... ...                           |
	|struct fasync_struct *fasync_list;|       +----------------------------------+
	|... ...                           |       
	+----------------------------------+       
                                               +-----------------------------+
	+-----------------------+                  |struct file                  |
	|struct proto_ops       |                  +-----------------------------+
	+-----------------------+                  |struct file *f_next, *f_prev;|
	|int	family();       |                  |struct inode *f_inode;       |
	|int	(*create)();    |                  |struct file_operations *f_op;|
	|int	(*dup)();       |                  |... ...                      |
	|int	(*release)();   |                  +-----------------------------+
	|int	(*bind)();      |                  
	|int	(*connect)();   |                  +-----------------------------+
	|int	(*socketpair)();|                  |struct file_operations       |
	|int	(*accept)();    |                  +-----------------------------+
	|int	(*getname)();   |                  |int (*open) ();              |
	|int	(*read)();      |                  |int (*read) ();              |
	|int	(*write)();     |                  |int (*write) ();             |
	|int	(*select)();    |                  |int (*select) ();            |
	|int	(*ioctl)();     |                  |int (*release) ();           |
	|int	(*listen)();    |                  |... ...                      |
	|int	(*send)();      |                  +-----------------------------+
	|int	(*recv)();      |
	|int	(*sendto)();    |                  +------------------------------+
	|int	(*recvfrom)();  |                  |struct inode                  |
	|int	(*shutdown)();  |                  +------------------------------+
	|int	(*setsockopt)();|                  |struct inode *i_next, *i_prev;|
	|int	(*getsockopt)();|                  |struct inode_operations *i_op;|
	|int	(*fcntl)();     |                  |... ...                       |
	+-----------------------+                  +------------------------------+

