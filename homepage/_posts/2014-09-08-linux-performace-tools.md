---
layout: post
title: "linux performace tools"
description: ""
category: tools
tags: [performance]
---

###

![linux benchmarking tools](/assets/image/posts/perf/linux_benchmarking_tools.png)

![linux observability sar](/assets/image/posts/perf/linux_observability_sar.png)

![linux tuning tools](/assets/image/posts/perf/linux_tuning_tools.png)


### Tools: Basic
* uptime
* top or htop
* mpstat
* iostat
* vmstat
* free 
* ping
* nicstat
* dstat

### Tools: Intermediate
* sar
* netstat
* pidstat
* strace
* tcpdump
* blktrace
* iotop
* slabtop
* sysctl
* /proc

### Tools: Advanced
* perf
  - `perf record program [program_option]`
  - `perf report`
* DTrace
* SystemTap
* and more...
  - ps
  - pmap
  - traceroute
  - ntop
  - ss
  - lsof
  - oprofile
  - gprof
  - kcachegrind
  - valgrind
  - google profiler
  - nfsiostat
  - cifsiostat
  - latencytop
  - powertop
  - LLTng
  - ktap

### Reference
* [Linux Performance](http://www.brendangregg.com/linuxperf.html)
* [Linux performance analysis and tools](http://www.slideshare.net/brendangregg/linux-performance-analysis-and-tools)
* [Tag: performance](http://www.vpsee.com/tag/performance/)
* [perf wiki](https://perf.wiki.kernel.org/index.php/Tutorial)
