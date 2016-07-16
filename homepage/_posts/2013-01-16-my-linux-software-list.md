---
layout: post
title: "List my linux software"
description: ""
category: person
tags: []
---

### 更新历史
<table class="table">
   <tr>
      <td>日期</td>
      <td>版本</td>
      <td>详细</td>
   </tr>
   <tr>
      <td>2015-01-26</td>
      <td>1.3</td>
      <td>FC21</td>
   </tr>
   <tr>
      <td>2014-06-11</td>
      <td>1.2</td>
      <td>FC20</td>
   </tr>
   <tr>
      <td>2013-01-19</td>
      <td>1.1</td>
      <td>加入前序,排版,增加项目</td>
   </tr>
   <tr>
      <td>2013-01-16</td>
      <td>1.0</td>
      <td>第一版, FC18</td>
   </tr>
</table>

### 使用历史
约在04年左右开始接触Linux，印象中使用的应该是国内的红旗;
但后续的日子，基本上没有再接触了. 直到在上海工作，闲来无事，在网上写了个地址，然
后收到张A国寄来的Ubuntu光盘，就又重新接触了，大约是08年吧；之后有使用过Fedora，
工作中也使用过centos(主要用作编译服务器，ftp服务，版本仓库),其他的也有用过，
如Mint(作Linux kernel编译玩具)，BackTrack(用于信息安全方面的系统，基本不会用);
近半年来，回了南方，换了工作，有机会全身心投入Linux世界，于是乎把公司以及家里的
系统kill掉，全换成了Fedora17,前天升级到fc18.

这里罗列Linux(Fedora)下常用的一些工具，主要作为备忘，别无他目.

### 工具列表

* 远程桌面: rdesktop, teamviewer

* 抓包工具: wireshark

* 虚拟机: Virtual Box、kvm

* 办公软件: LibOffice(浏览工作中的文档)

* 浏览器: Firefox(插件vimperator,autoproxy)、Chrome(插件vimium,TunnelSwitch)

* 邮件: Thunderbird

* 文件管理: gnome-commander(类似于windows下的TotalCommander,但差很多)

* 版本控制: git(写blog和查看开源代码时使用)、svn(工作时使用)

* 代码编辑器: VIM、Emacs

* 十六进制文件编辑: bless, hexdump -C, vim -b

* 笔记: Emacs org mode, markdown

* 流程图: graphviz dot (其他工具 PlantUML bouml)

* 博客: github+jekyll

* 下载: wget

* 墙外: ssh or goagent + firefox autoproxy, 鲨鱼VPN

* 数学软件: maxima

* 翻译: stardict

* IRC: irssi、xchat

* 即时通(IM): skype

* RSS: greader、liferea

* 文档转换: pandoc(convert documents to pdf)

* 图片编辑: gimg shutter ImageMagick

* 视频播放: smplayer

* 其他:  系统监测-Conky 

### Install on FC20

    # 2014-06-11 Create by Dennis

	yum install -y rdesktop

    # for gvim
    yum install -y vim-X11 vim-common vim-enhanced vim-minimal
    # for vim
    yum install -y cscope ctags 

    # for git svn g++  
    yum install -y git svn gcc-c++ 

    # for thunderbird, download the rpm package from website
    #yum install thunderbird-24.5.0-1.fc20.x86_64.rpm 
    yum install -y thunderbird

    # for jekyll  
    yum install -y ruby-devel
    # it will take several minutes to finish
    gem install jekyll
    gem install rake
    # for js
    yum install -y nodejs

    # for graphviz
    yum install -y graphviz

    # for teamviewer, download rpm package from website
    # yum install teamviewer_linux.rpm 

    yum install sqlite-devel

    yum install clang

    # for dict
    yum install -y stardict

    # for ImageMagick
    yum install -y ImageMagick

For pandoc generate pdf   

    yum install pandoc 
    yum -y install texlive texlive-latex texlive-xetex
    yum -y install texlive texlive-latex texlive-xetex
    yum -y install texlive-collection-latex
    yum -y install texlive-collection-latexrecommended
    yum -y install texlive-xetex-def
    yum -y install texlive-collection-xetex
    #Only if needed:
    #yum install texlive-collection-latexextra
    yum install texlive-titlesec
    yum install texlive-titling
    yum install texlive-lastpage
    yum install "tex(eu1enc.def)"
    yum install texlive-xetex-def
    yum install texlive-xecjk

Install virtualbox  

    # for virtualbox, download rpm package from website
    yum install VirtualBox-4.3-4.3.12_93733_fedora18-1.x86_64.rpm
    yum install dkms
    /etc/init.d/vboxdrv setup

    # **trouble shoot**
    #
    # [root@indigo kernel]# /etc/init.d/vboxdrv setup  
    # Stopping VirtualBox kernel modules                         [  OK  ]  
    # Uninstalling old VirtualBox DKMS kernel modules            [  OK  ]  
    # Trying to register the VirtualBox kernel modules using DKMSError! echo  
    # Your kernel headers for kernel 3.14.5-200.fc20.x86_64 cannot be found at  
    # /lib/modules/3.14.5-200.fc20.x86_64/build or /lib/modules/3.14.5-200.fc20.x86_64/source.  
    #                                                            [FAILED]  
    #   (Failed, trying without DKMS)  
    # Recompiling VirtualBox kernel modules                      [FAILED]  
    #   (Look at /var/log/vbox-install.log to find out what went wrong)
    #
    yum instal kernel
    yum instal kernel-devel
    reboot
    # then select the latest kernel to boot the os

    # add dennis to vboxusers group, support virtualbox USB (default root)
    gpasswd -a dennis vboxusers
    [dennis@localhost ~]$ groups $USER
    dennis : dennis vboxusers

Install rar   

    wget -c http://www.rarsoft.com/rar/rarlinux-x64-5.1.0.tar.gz
    tar xvf rarlinux-x64-5.1.0.tar.gz
    cd rar
    make

Install smplayer  

    rpm -ivh http://rpm.livna.org/livna-release.rpm
    rpm -Uvh http://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-stable.noarch.rpm
    rpm -Uvh http://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-stable.noarch.rpm
    yum -y install smplayer
    yum -y install mplayer-gui

### 参考链接
+ [The Linux Alternative Project](http://www.linuxalt.com/)
+ [Fedora安装smplayer和mplayer](http://www.chenxuanyi.cn/fedora-smplayer-mplayer.html)
