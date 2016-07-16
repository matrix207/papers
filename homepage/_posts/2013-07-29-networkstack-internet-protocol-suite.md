---
layout: post
title: "NetworkStack Internet protocol suite"
description: ""
category: network
tags: [network]
---

### 1. Introduce
The Internet protocol suite and the layered protocol stack design were in use 
before the OSI model was established. Since then, the TCP/IP model has been 
compared with the OSI model in books and classrooms, which often results in 
confusion because the two models use different assumptions and goals, including 
the relative importance of strict layering.

### 2. TCP/IP Network model
Layers in the internet protocol suite:( a four-layer model )  

	+------------------------+                         +--------+
	|    Application layer   |                         |  Data  |
	+------------------------+                         +--------+
	            |
	  +--------------------+                   +-------+--------+
	  |   Transport layer  |                   |TCP/IP |  Data  |
	  +--------------------+                   +-------+--------+
	            |
	  +-------------------+              +-----+-------+--------+
	  |   Internet layer  |              | IP  |TCP/IP |  Data  |
	  +-------------------+              +-----+-------+--------+
	            |
	    +---------------+       +--------+-----+-------+--------+
	    |   Link layer  |       | Header | IP  |TCP/IP |  Data  |
	    +---------------+       +--------+-----+-------+--------+

### Protocol in layers
Application layer

    DHCP
    DHCPv6
    DNS
    FTP
    HTTP*
    IMAP
    IRC
    LDAP
    MGCP
    NNTP
    BGP
    NTP
    POP
    RPC
    RTP
    RTSP
    RIP
    SIP
    SMTP
    SNMP*
    SOCKS
    SSH
    Telnet
    TLS/SSL
    XMPP

Transport layer

    TCP*
    UDP*
    DCCP
    SCTP
    RSVP

Internet layer

    IP(IPv4,IPv6)*
    ICMP*
    ICMPv6
    ECN
    IGMP
    IPsec

Link layer

    ARP/InARP*
    NDP
    OSPF
    Tunnels(L2TP)
    PPP*
    Media access control(Ethernet/DSL/ISDN/FDDI)

### 3. Protocol Format
IP - <http://www.ietf.org/rfc/rfc791.txt>, page 11  
TCP - <http://www.ietf.org/rfc/rfc793.txt>, page 15  
UDP - <http://www.ietf.org/rfc/rfc768.txt>, page 1  

	0                   1                   2                   3   
	0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 
	+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
	|Version|  IHL  |Type of Service|          Total Length         |
	+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
	|         Identification        |Flags|      Fragment Offset    |
	+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
	|  Time to Live |    Protocol   |         Header Checksum       |
	+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
	|                       Source Address                          |
	+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
	|                    Destination Address                        |
	+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
	|                    Options                    |    Padding    |
	+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

	              Example Internet Datagram Header

	0                   1                   2                   3   
	0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 
	+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
	|          Source Port          |       Destination Port        |
	+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
	|                        Sequence Number                        |
	+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
	|                    Acknowledgment Number                      |
	+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
	|  Data |           |U|A|P|R|S|F|                               |
	| Offset| Reserved  |R|C|S|S|Y|I|            Window             |
	|       |           |G|K|H|T|N|N|                               |
	+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
	|           Checksum            |         Urgent Pointer        |
	+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
	|                    Options                    |    Padding    |
	+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
	|                             data                              |
	+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

                            TCP Header Format

	0      7 8     15 16    23 24    31  
	+--------+--------+--------+--------+ 
	|     Source      |   Destination   | 
	|      Port       |      Port       | 
	+--------+--------+--------+--------+ 
	|                 |                 | 
	|     Length      |    Checksum     | 
	+--------+--------+--------+--------+ 
	|                                     
	|          data octets ...            
	+---------------- ...                 

	     User Datagram Header Format

### 4. Protocol links
IP - Internet Protocol  
<http://www.ietf.org/rfc/rfc791.txt>

ARP - Address Resolution Protocol  
<http://www.ietf.org/rfc/rfc826.txt>  
<http://en.wikipedia.org/wiki/Arp_protocol>

ICMP - Internet Control Message Protoco  
<http://en.wikipedia.org/wiki/Internet_Control_Message_Protocol>  
<http://www.ietf.org/rfc/rfc792.txt>

TCP - Transmission Control Protocol  
<http://en.wikipedia.org/wiki/Transmission_Control_Protocol>  
<http://www.ietf.org/rfc/rfc793.txt>

SMTP - Simple Mail Transfer Protocol  
<http://www.ietf.org/rfc/rfc821.txt>

FTP - File Transfer Protocol  
<http://www.ietf.org/rfc/rfc958.txt>

SNMP - Simple Network Management Protocol  
<http://www.ietf.org/rfc/rfc1067.txt>

IGMP - Internet Group Management Protocol  
<http://www.ietf.org/rfc/rfc3376.txt>  
<http://en.wikipedia.org/wiki/Internet_Group_Management_Protocol>

UDP - User Datagram Protocol  
<http://en.wikipedia.org/wiki/User_Datagram_Protocol>  
<http://www.ietf.org/rfc/rfc768.txt>

PPP - Point to point protocol  
<http://en.wikipedia.org/wiki/Point-to-point_protocol>  
<http://tools.ietf.org/html/rfc1661>

SMTP - Simple Mail Transfer Protocol  
<http://en.wikipedia.org/wiki/Simple_Mail_Transfer_Protocol>  

HTTP - Hypertext Transfer Protocol  
<http://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol>  

### 5. Reference  
+ [Internet protocol suite](http://en.wikipedia.org/wiki/Internet_protocol_suite)
+ [IP/IP协议](http://zh.wikipedia.org/wiki/TCP/IP协议)
+ [TCP-IP](http://www.linux-tutorial.info/modules.php?name=MContent&obj=page&pageid=142)
+ [TCP/IP and IMS Sequence Diagrams](http://www.eventhelix.com/RealtimeMantra/Networking/#IP_-_Internet_Protocol)
