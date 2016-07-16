---
layout: post
title: "optimal file system with tune2fs"
description: ""
category: storage
tags: [filesystem]
---

### view filesystem information
	dennis@dennis:~$ sudo su
	[sudo] password for dennis: 
	root@dennis:/home/dennis# fdisk -l

	Disk /dev/sda: 1000.2 GB, 1000204886016 bytes
	255 heads, 63 sectors/track, 121601 cylinders, total 1953525168 sectors
	Units = sectors of 1 * 512 = 512 bytes
	Sector size (logical/physical): 512 bytes / 4096 bytes
	I/O size (minimum/optimal): 4096 bytes / 4096 bytes
	Disk identifier: 0x000ee377

	   Device Boot      Start         End      Blocks   Id  System
	/dev/sda1   *     1026048     1230847      102400    7  HPFS/NTFS/exFAT
	/dev/sda2         1230848   210741247   104755200    7  HPFS/NTFS/exFAT
	/dev/sda3       210741248   860366847   324812800    7  HPFS/NTFS/exFAT
	/dev/sda4       860368894  1953523711   546577409    5  Extended
	Partition 4 does not start on physical sector boundary.
	/dev/sda5       860368896  1937033215   538332160   83  Linux
	/dev/sda6      1937035264  1953523711     8244224   82  Linux swap / Solaris
	root@dennis:/home/dennis# tune2fs -l /dev/sda1
	tune2fs 1.42 (29-Nov-2011)
	tune2fs: Bad magic number in super-block while trying to open /dev/sda1
	Couldn't find valid filesystem superblock.
	root@dennis:/home/dennis# tune2fs -l /dev/sda6
	tune2fs 1.42 (29-Nov-2011)
	tune2fs: Bad magic number in super-block while trying to open /dev/sda6
	Couldn't find valid filesystem superblock.
	root@dennis:/home/dennis# tune2fs -l /dev/sda5
	tune2fs 1.42 (29-Nov-2011)
	Filesystem volume name:   <none>
	Last mounted on:          /
	Filesystem UUID:          cab5dcf8-904f-4f53-a2f8-25276e38f84d
	Filesystem magic number:  0xEF53
	Filesystem revision #:    1 (dynamic)
	Filesystem features:      has_journal ext_attr resize_inode dir_index filetype needs_recovery extent flex_bg sparse_super large_file huge_file uninit_bg dir_nlink extra_isize
	Filesystem flags:         signed_directory_hash 
	Default mount options:    user_xattr acl
	Filesystem state:         clean
	Errors behavior:          Continue
	Filesystem OS type:       Linux
	Inode count:              33652736
	Block count:              134583040
	Reserved block count:     6729152
	Free blocks:              112706967
	Free inodes:              33202652
	First block:              0
	Block size:               4096
	Fragment size:            4096
	Reserved GDT blocks:      991
	Blocks per group:         32768
	Fragments per group:      32768
	Inodes per group:         8192
	Inode blocks per group:   512
	Flex block group size:    16
	Filesystem created:       Thu Mar 27 19:19:17 2014
	Last mount time:          Sat Aug  9 13:51:31 2014
	Last write time:          Wed Apr  9 11:19:12 2014
	Mount count:              94
	Maximum mount count:      -1
	Last checked:             Thu Mar 27 19:19:17 2014
	Check interval:           0 (<none>)
	Lifetime writes:          174 GB
	Reserved blocks uid:      0 (user root)
	Reserved blocks gid:      0 (group root)
	First inode:              11
	Inode size:               256
	Required extra isize:     28
	Desired extra isize:      28
	Journal inode:            8
	First orphan inode:       18089038
	Default directory hash:   half_md4
	Directory Hash Seed:      d4c17cf0-bb57-4423-81ab-a8fc3df1bc13
	Journal backup:           inode blocks

### Terminology 
The block size is the unit of work for the file system. Every read and write is 
done in full multiples of the block size. The block size is also the smallest 
size on disk a file can have.

### Optimal
1.Inode block

2.Reserved block

	Mkfs.ext3 –b 4096 - i 8192 –m 2 /dev/sda5

3.tune2fs

	dennis@dennis:~$ tune2fs
	tune2fs 1.42 (29-Nov-2011)
	Usage: tune2fs [-c max_mounts_count] [-e errors_behavior] [-g group]
		[-i interval[d|m|w]] [-j] [-J journal_options] [-l]
		[-m reserved_blocks_percent] [-o [^]mount_options[,...]] [-p mmp_update_interval]
		[-r reserved_blocks_count] [-u user] [-C mount_count] [-L volume_label]
		[-M last_mounted_dir] [-O [^]feature[,...]]
		[-E extended-option[,...]] [-T last_check_time] [-U UUID]
		[ -I new_inode_size ] device

use `tune2fs -l /dev/sda5` to view filesystem information

use `tune2fs -c -1 /dev/sda5` to avoid selfcheck

use `tune2fs -c -1 -i 0 /dev/sda5` to set the interval time

use `tune2fs -m 3 /dev/sda5` to set the reserved_blocks_percent

### Reference
* [优化linux文件系统 提高读写速度](http://linux.chinaitlab.com/administer/824245.html)
