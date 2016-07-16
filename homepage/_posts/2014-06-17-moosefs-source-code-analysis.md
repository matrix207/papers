---
layout: post
title: "moosefs source code analysis"
description: ""
category: source
tags: [filesystem]
---

### mfs-1.5.9
1. Files

    main.c ---> Init,   
    init.c ---> defined a `RunTab`, use for mainloop
	changelog.c  changelog_init change log
	random.c rndinit  random generator
	matocsserv.c matocsserv_init  communication with chunkserver
	matocuserv.c matocuserv_init  communication with customer
	filesystem.c fs_init  file system manager
	stats.c stats_init  statistics module

    [dennis@localhost mfs-1.5.9]$ ls
    aclocal.m4    config.sub    COPYING  INSTALL      Makefile.in         
    config.guess  configure     depcomp  install-sh   mfschunkserver      
    config.h.in   configure.ac  doc      Makefile.am  MFSCommunication.h  
    mfsdata       mfsmount      README   mfsmaster    missing mfsmetarestore  
    NEWS

2. TODO

#### master server
1. Files

    [dennis@localhost mfs-1.5.9]$ ls mfsmaster/
    cfg.c        changelog.h  filesystem.h  lempelziv.h  Makefile.am   
    matocsserv.h random.c     sockets.h     cfg.h        chunks.c     
    datapack.h   init.h       main.c        Makefile.in   matocuserv.c  
    changelog.c  chunks.h     filesystem.c  lempelziv.c   main.h       
    matocsserv.c  matocuserv.h  sockets.c  stats.h        random.h   stats.c

#### chunk  server
1. TODO

#### client
1. TODO

### Reference 
* [MooseFS HomePage](http://www.moosefs.org/download.html)
* <http://sourceforge.net/projects/moosefs/>
* <http://sourceforge.net/projects/moosefs/files/moosefs/>
* [MooseFS introduce](http://www.cnblogs.com/JeffreySun/archive/2011/04/03/2004255.html)
* [mfsmount module](http://www.cnblogs.com/JeffreySun/archive/2011/04/03/2004259.html)
* [mfsmaster module](http://www.cnblogs.com/JeffreySun/archive/2011/04/03/2004260.html)
