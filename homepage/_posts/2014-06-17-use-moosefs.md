---
layout: post
title: "use moosefs"
description: ""
category: program
tags: [filesystem]
---

### Master server installation

* **Installation**

        groupadd mfs
        useradd -g mfs mfs
        cd /usr/src
        tar -zxvf mfs-1.6.15.tar.gz
        cd mfs-1.6.15
        ./configure --prefix=/usr --sysconfdir=/etc --localstatedir=/var/lib 
        --with-default-user=mfs --with-default-group=mfs --disable-mfschunkserver 
        --disable-mfsmount
        make
        make install 

* **Configuration**

1. Copy config

        cd /etc
        cp mfsmaster.cfg.dist mfsmaster.cfg
        cp mfsmetalogger.cfg.dist mfsmetalogger.cfg
        cp mfsexports.cfg.dist mfsexports.cfg

        # modify mfsexports.cfg 
        # 192.168.2.0/24 / rw,alldirs,maproot=0 

2. Copy meta data file

        cd /var/lib/mfs
        cp metadata.mfs.empty metadata.mfs

3. Config host name

        echo 192.168.1.1 mfsmaster>> /etc/hosts

* **Run server**

        /usr/sbin/mfsmaster start

        # Run CGI monitor server, and view info under <http://192.168.1.1:9425>
        /usr/sbin/mfscgiserv 


### Install Backup Server (metalogger)

Metalogger installation is very similar to that of the master server. 
We issue the following commands:

    groupadd mfs
    useradd -g mfs mfs
    cd /usr/src
    tar -zxvf mfs-1.6.15.tar.gz
    cd mfs-1.6.15
    ./configure --prefix=/usr --sysconfdir=/etc --localstatedir=/var/lib 
    --with-default-user=mfs --with-default-group=mfs --disable-mfschunkserver 
    --disable-mfsmount
    make
    make install

    cd /etc
    cp mfsmetalogger.cfg.dist mfsmetalogger.cfg

Similarly, in /etc/hosts we add:

    192.168.1.1 mfsmaster

Now we are ready to start the backup server process :

    /usr/sbin/mfsmetalogger start

In a production environment you should set up automatic start of mfsmetalogger. 


### Chunk servers installation

We issue the following commands on the machines which are to be chunks servers:

    groupadd mfs
    useradd -g mfs mfs
    cd /usr/src
    tar -zxvf mfs-1.6.15.tar.gz
    cd mfs-1.6.15
    ./configure --prefix=/usr --sysconfdir=/etc --localstatedir=/var/lib 
    --with-default-user=mfs --with-default-group=mfs --disable-mfsmaster
    make
    make install

Now we similarly prepare configuration files of the chunk server:

    cd /etc/
    cp mfschunkserver.cfg.dist mfschunkserver.cfg
    cp mfshdd.cfg.dist mfshdd.cfg 

use `/mnt/mfschunks1` and `/mnt/mfschunks2` locations , so we add these two 
lines to mfshdd.cfg file:

    /mnt/mfschunks1
    /mnt/mfschunks2

Create a `.lock` file to make user `mfs` has rights to write the mounted partitions.

    chown -R mfs:mfs /mnt/mfschunks1
    chown -R mfs:mfs /mnt/mfschunks2 

Similarly we add the following line in `/etc/hosts` file:

    192.168.1.1 mfsmaster

Now we are ready to start the chunk server:

    /usr/sbin/mfschunkserver start 

### Users’ computers installation  
Install FUSE

    cd /usr/src
    tar -zxvf fuse-2.8.3.tar.gz
    cd fuse-2.8.3
    ./configure
    make
    make install

In order to install mfsmount package we do the following steps:

    tar -zxvf mfs-1.6.15.tar.gz
    cd mfs-1.6.15
    ./configure --prefix=/usr --sysconfdir=/etc
    --localstatedir=/var/lib --with-default-user=mfs
    --with-default-group=mfs --disable-mfsmaster
    --disable-mfschunkserver
    make
    make install

In a /etc/hosts file we add this line:

    192.168.1.1 mfsmaster

Let’s assume that we will mount the system in a /mnt/mfs folder on a client’s 
machine. We issue the following commands:

    mkdir -p /mnt/mfs
    /usr/bin/mfsmount /mnt/mfs -H mfsmaster

Now after issuing the `df -h | grep mfs` command we should get information 
similar to this:

    /storage/mfschunks/mfschunks1
        2.0G 69M 1.9G 4% /mnt/mfschunks1
    /storage/mfschunks/mfschunks2
        2.0G 69M 1.9G 4% /mnt/mfschunks2
    mfs#mfsmaster:9421 3.2G 0 3.2G 0% /mnt/mfs 

### Installing MooseFS on one server 
* Install `FUSE`
* Install `Moosefs`

### Basic MooseFS Usage 
The number of copies for the folder is set with the `mfssetgoal –r` command: 

    mfssetgoal -r 1 /mnt/mfs/folder1
    /mnt/mfs/folder1:
    inodes with goal changed: 0
    inodes with goal not changed: 1
    inodes with permission denied: 0
    mfssetgoal -r 2 /mnt/mfs/folder2
    /mnt/mfs/folder2:
    inodes with goal changed: 0
    inodes with goal not changed: 1
    inodes with permission denied: 0 

The command `mfscheckfile` is used for checking in how many copies the given 
file is stored. 

### Reference
* <http://www.moosefs.org/tl_files/manpageszip/moosefs-step-by-step-tutorial-v.1.1.pdf>
* <http://www.moosefs.org/tl_files/manpageszip/moosefs-step-by-step-tutorial-cn-v.1.1.pdf>
* <http://sourceforge.net/projects/moosefs/files/moosefs/>
* <http://bbs.chinaunix.net/thread-1644309-1-1.html>
* <http://bbs.chinaunix.net/thread-1643863-1-1.html>
* <http://sery.blog.51cto.com/10037/263515>
