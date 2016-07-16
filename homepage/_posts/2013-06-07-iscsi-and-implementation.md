---
layout: post
title: "iSCSI and implementation"
description: ""
category: storage
tags: [storage,iscsi]
---

### Introduce
iSCSI（发音为 /аɪskʌzi/）（iSCSI = internet Small Computer System Interface ）
又稱為IP-SAN，是一种基于因特网及SCSI-3协议下的存储技术，
由IETF提出，並於2003年2月11日成為正式的標準。
与传统的SCSI技术比较起来，iSCSI技术有以下三个革命性的变化：

	1. 把原來只用於本機的SCSI協同透過TCP/IP网络傳送，使连接距离可作无限的地域延伸
	2. 连接的服务器数量无限（原來的SCSI-3的上限是15）
	3. 由于是服务器架构，因此也可以实现在线扩容以至动态部署

iSCSI利用了TCP/IP的port 860 和 3260 作为沟通的渠道。透过两部计算机之间利用iSCSI
的协议来交换SCSI命令，让计算机可以透过高速的局域网集线来把SAN模拟成为本地的储存装置。

The overall structure of an iSCSI  PDU is as follows:

	Byte/     0       |       1       |       2       |       3       |
	   /              |               |               |               |
	  |0 1 2 3 4 5 6 7|0 1 2 3 4 5 6 7|0 1 2 3 4 5 6 7|0 1 2 3 4 5 6 7|
	  +---------------+---------------+---------------+---------------+
	 0/ Basic Header Segment (BHS)                                    /
	 +/                                                               /
	  +---------------+---------------+---------------+---------------+
	48/ Additional Header Segment 1 (AHS)  (optional)                 /
	 +/                                                               /
	  +---------------+---------------+---------------+---------------+
	  / Additional Header Segment 2 (AHS)  (optional)                 /
	 +/                                                               /
	  +---------------+---------------+---------------+---------------+
	 ----
	  +---------------+---------------+---------------+---------------+
	  / Additional Header Segment n (AHS)  (optional)                 /
	 +/                                                               /
	  +---------------+---------------+---------------+---------------+
	 ----
	  +---------------+---------------+---------------+---------------+
	 k/ Header-Digest (optional)                                      /
	 +/                                                               /
	  +---------------+---------------+---------------+---------------+
	 l/ Data Segment(optional)                                        /
	 +/                                                               /
	  +---------------+---------------+---------------+---------------+
	 m/ Data-Digest (optional)                                        /
	 +/                                                               /
	  +---------------+---------------+---------------+---------------+


The format of the BHS(fixed size, 48 bytes ) is:

	Byte/     0       |       1       |       2       |       3       |
	   /              |               |               |               |
	  |0 1 2 3 4 5 6 7|0 1 2 3 4 5 6 7|0 1 2 3 4 5 6 7|0 1 2 3 4 5 6 7|
	  +---------------+---------------+---------------+---------------+
	 0|.|I| Opcode    |F|  Opcode-specific fields                     |
	  +---------------+---------------+---------------+---------------+
	 4|TotalAHSLength | DataSegmentLength                             |
	  +---------------+---------------+---------------+---------------+
	 8| LUN or Opcode-specific fields                                 |
	  +                                                               +
	12|                                                               |
	  +---------------+---------------+---------------+---------------+
	16| Initiator Task Tag                                            |
	  +---------------+---------------+---------------+---------------+
	20/ Opcode-specific fields                                        /
	 +/                                                               /
	  +---------------+---------------+---------------+---------------+
	48

For request PDUs, the I bit set to 1 is an immediate delivery marker.

Initiator opcodes defined in this specification are:

	 0x00 NOP-Out
	 0x01 SCSI Command (encapsulates a SCSI Command Descriptor Block)
	 0x02 SCSI Task Management function request
	 0x03 Login Request
	 0x04 Text Request
	 0x05 SCSI Data-Out (for WRITE operations)
	 0x06 Logout Request
	 0x10 SNACK Request
	 0x1c-0x1e Vendor specific codes

Target opcodes are:

     0x20 NOP-In
     0x21 SCSI Response - contains SCSI status and possibly sense
          information or other response information.
     0x22 SCSI Task Management function response
     0x23 Login Response
     0x24 Text Response
     0x25 SCSI Data-In - for READ operations.
     0x26 Logout Response
     0x31 Ready To Transfer (R2T) - sent by target when it is ready
          to receive data.
     0x32 Asynchronous Message - sent by target to indicate certain
          special conditions.
     0x3c-0x3e Vendor specific codes
     0x3f Reject

When set to 1 it indicates the final (or only) PDU of a sequence.

Some opcodes operate on a specific Logical Unit.  The Logical Unit
Number (LUN) field identifies which Logical Unit.  If the opcode does
not relate to a Logical Unit, this field is either ignored or may be
used in an opcode specific way.  The LUN field is 64-bits and should
be formatted in accordance with \[SAM2\].  For example, LUN\[0\] from
\[SAM2\] is BHS byte 8 and so on up to LUN\[7\] from \[SAM2\], which is BHS
byte 15.

The general format of an AHS is:

	Byte/     0       |       1       |       2       |       3       |
	   /              |               |               |               |
	  |0 1 2 3 4 5 6 7|0 1 2 3 4 5 6 7|0 1 2 3 4 5 6 7|0 1 2 3 4 5 6 7|
	  +---------------+---------------+---------------+---------------+
	 0| AHSLength                     | AHSType       | AHS-Specific  |
	  +---------------+---------------+---------------+---------------+
	 4/ AHS-Specific                                                  /
	 +/                                                               /
	  +---------------+---------------+---------------+---------------+
	 x


The format of the SCSI Command PDU is:

	Byte/     0       |       1       |       2       |       3       |
	   /              |               |               |               |
	  |0 1 2 3 4 5 6 7|0 1 2 3 4 5 6 7|0 1 2 3 4 5 6 7|0 1 2 3 4 5 6 7|
	  +---------------+---------------+---------------+---------------+
	 0|.|I| 0x01      |F|R|W|. .|ATTR | Reserved                      |
	  +---------------+---------------+---------------+---------------+
	 4|TotalAHSLength | DataSegmentLength                             |
	  +---------------+---------------+---------------+---------------+
	 8| Logical Unit Number (LUN)                                     |
	  +                                                               +
	12|                                                               |
	  +---------------+---------------+---------------+---------------+
	16| Initiator Task Tag                                            |
	  +---------------+---------------+---------------+---------------+
	20| Expected Data Transfer Length                                 |
	  +---------------+---------------+---------------+---------------+
	24| CmdSN                                                         |
	  +---------------+---------------+---------------+---------------+
	28| ExpStatSN                                                     |
	  +---------------+---------------+---------------+---------------+
	32/ SCSI Command Descriptor Block (CDB)                           /
	 +/                                                               /
	  +---------------+---------------+---------------+---------------+
	48/ AHS (Optional)                                                /
	  +---------------+---------------+---------------+---------------+
	 x/ Header Digest (Optional)                                      /
	  +---------------+---------------+---------------+---------------+
	 y/ (DataSegment, Command Data) (Optional)                        /
	 +/                                                               /
	  +---------------+---------------+---------------+---------------+
	 z/ Data Digest (Optional)                                        /
	  +---------------+---------------+---------------+---------------+


### Reference:  
+ [iSCSI](http://en.wikipedia.org/wiki/ISCSI)
+ [RFC 3720 TEXT format](http://www.ietf.org/rfc/rfc3720.txt)
+ [RFC 3720 HTML format](http://tools.ietf.org/html/rfc3720)
+ [iSCSI存储技术全攻略](http://www.sansky.net/article/2007-12-03-iscsi-storage.html)
+ [iscsitarget](http://sourceforge.net/projects/iscsitarget/)
+ [pyTarget](http://sourceforge.net/projects/pytarget/)
+ [jSCSI is a cross-platform Java implementation of an iSCSI initiator](http://jscsi.sourceforge.net/)
+ [jSCSI on github](https://github.com/disy/jSCSI)
+ [TGT](http://stgt.sourceforge.net/)
+ [Slim PHP iSCSI Panel for Centos](http://slimphpiscsipan.sourceforge.net)

