---
layout: post
title: "iSCSI Enterprise Target"
description: ""
category: storage 
tags: [iscsi]
---

iSCSI Enterprise Target
-----------------------

### Introduce

    SCSI     : small computer system interface
    iSCSI    : Internet SCSI
    Initiator:
    Target   :

### component of protocol
1. Communication
    * connect 
    * session
      - login
      - full feature
    * pdu

2. Naming and addressing
    * iqn.iSCSI 
    * eui.IEEEEUI264

3. Error Handle
    * misclassification
    * Recovery mechanism

4. Security
    * Authentication
      - CHAP
    
### Code flow
1. Login phase
2. Data execute of text phase
3. logout pahse
   * Report LUNS(0xa0)-> 
   * Inquiry(0x12) -> 
   * Read Capacity(10) (0x25) -> 
   * Read(10)(0x28) -> 
   * Mode Sense(6)(0x1a)

### Source code
* source:

* build:

* patch

        tar xvf iscsitarget-1.4.20.tar.gz
        cd iscsitarget-1.4.20
        patch -p1 < ../iscsitarget-1.4.20_patch/compat-2.6.32.patch
        patch -p1 < ../iscsitarget-1.4.20_patch/iscsitarget-xxx-1.4.20_for_centos_6_2.patch
        patch -p1 < ../iscsitarget-1.4.20_patch/iscsi_conn_close_raid_failed.patch
        patch -p1 < ../iscsitarget-1.4.20_patch/iscsi_load_balance_bug_fix.patch
        patch -p1 < ../iscsitarget-1.4.20_patch/iscsi_flow_control.patch
        patch -p1 < ../iscsitarget-1.4.20_patch/iscsi_disable_sync_cache.patch

### Analysis

#### Kernel module

iscsi_trgt.ko

most of the work were done in kernel, such as data send and recv, 
and the user mode do session create, author and login.

sub-module list as below:

* Files
    * iotype.c
    * nthread.c
      - listen iSCSI request from iSCSI Initiator
      - receive iSCSI request
      - initialize iSCSI Command structure and LUN parameters
      - allocate data cache (struct tio)
      - handle r2t data
    * wthread.c
      - communicate with nthread
      - handle iSCSI command from nthread
      - command can be divided into two types: related , not related 
      - execute callback function
    * response
      - dispatch by wthread

#### User module

ietd daemon, manager tool for IET, function description:

* configuration, such as port, uid, gid and so on.
* read config info from /etc/ietd.conf, and send to kernel module 

#### iSCSI mod
* blockio
  translate network data to struct tio, then handle by 
  blockio_make_request from block_io.c, submit_bio to 
  common layer after generate bio 
* fileio
  still use VFS, and do system call read/write to operate data.
* functional description  

### How IET add lun?
加入volume有两种方式:
1. 通过配置文件ietd.conf
2. 通过ietdadm命令，使用`--op new --tid=[id] --lun=[lun] --params Path=[path]`参数

第一种方式的流程如下: (用户态)  
`plain_init() -> __plain_lunit_create -> ki->lunit_create -> iscsi_lunit_create -> ioctl(ctrl_fd, ADD_VOLUME, &info)`

第二种方式的流程如下: (用户态)  
`ietadm_request_exec -> cops->lunit_add() -> plain_lunit_create() -> __plain_lunit_create `  后面同上

核态部分相同:  
+ `config.c:add_volume -> volume.c:volume_add -> volume.c:parse_volume_params` 
+ `volume.c:parse_volume_params` 函数检查发现是opt_iomode类型，  

### Type, IOMode and Sector Setting

Run by the ietadm command as below:  

- `ietadm --op new --tid=%d --lun=%d --params Path=/dev/%s/%s,IOMode=%s,Sector=%s,Type=%s`
  + **IOMode**: wb or wt or ro
  + **Sector**: 512 or 4096
  + **Type**: fileio or blockio

Function calling:

- `ietadm.c: main() -> lunit_handle() -> ietd_request() -> ietd_connect(),ietd_request_send(),ietd_response_recv()`
  + `ietd_connect() -> socket(AF_LOCAL, SOCK_STREAM, 0), connect() `
  + `ietd_request_send() -> write(fd, req, sizeof(*req))`
  + `ietd_request_recv() -> readv(), read() `
- `ietd.c: main() -> message.c: ietadm_request_listen() -> `
  + `ietadm_request_listen() -> socket(AF_LOCAL, SOCK_STREAM, 0), bind(), listen()`

Code analysis:  

- `ietadm --op new --tid= --lun= --params`
  + 调用 ietadm.c:lunit_handle函数 ->ietd_request函数 ->ietd_request_send函数
  + ietd.c的event_loop函数监听到有消息，执行message.c:ietadm_request_handle函数 
  + message.c:ietadm_request_handle函数 -> ietadm_request_exec函数  
	类型为C_LUNIT_NEW, 执行cops->lunit_add  
  + `plain.c:plain_lunit_create ->__plain_lunit_create -> ki->lunit_create` 
  + `ctldev.c:iscsi_lunit_create -> ioctl(ctrl_fd, ADD_VOLUME, &info)` 
  + `config.c:add_volume -> volume.c:volume_add -> volume.c:parse_volume_params` 
  + `volume.c:parse_volume_params` 函数检查发现是opt_iomode类型，  
	如果是ro类型执行SetLUReadonly(volume);  
	如果是wb类型执行SetLUWCache(volume);  
	如果是wt类型则类型不可用  
  + SetLUReadonly和SetLUWCache都在iscsi.h中定义，都只是修改struct iet_volume结构体中的flags的值。
  + 另外块大小blocksize保存在结构体struct iet_volume的blk_shift中,且块大小必须是  
	2的幂次方，大于等于512，小于4096. 注意保存的是2的幂的次数，即如果是1024，则blk_shift是10
  + 如果fileio,进入file-io.c:fileio_attach函数, 调用parse_fileio_params, 这里会忽略好些参数的值.  
	之用path参数会有处理。  
  + 如果blockio,进入block-io.c:blockio_attach函数, 调用parse_blockio_params, 这里会忽略好些参数的值.  
	之用path参数会有处理。  
  + file-io.c 和 block-io.c要好好看看，这个逻辑卷到底怎么创建的.

Summary:

- **总结:**
  + Type: **如果是fileio那么读写操作通过vfs请求完成，如果是blockio则读写操作直接构造bio请求**
  + IOMode: **如果是wb,	**
  + Sector

### Other iSCSI implement
* open-iscsi, [homepage](http://open-iscsi.org), 
  [source](https://github.com/mikechristie/open-iscsi), 
  [code analysis](http://blog.chinaunix.net/uid-14518381-id-3237994.html)

### Reference 
* [ISCSI Wikipedia](http://en.wikipedia.org/wiki/ISCSI)
* [iSCSITarget](http://iscsitarget.sourceforge.net)
* [iSCSITarget download](http://sourceforge.net/projects/iscsitarget/files/iscsitarget/)
* [iSCSI Enterprise Target的架构分析](http://blog.csdn.net/kidd_3/article/details/7670415)

