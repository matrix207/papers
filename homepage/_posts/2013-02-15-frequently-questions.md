---
layout: post
title: "frequently questions"
description: ""
category: linux
tags: [FAQ]
---

### 1. Adobe Flash Player 11.2 on Fedora 18/17  
1). Install Adobe YUM Repository RPM package  
`rpm -ivh http://linuxdownload.adobe.com/adobe-release/adobe-release-x86_64-1.0-1.noarch.rpm`  
`rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-adobe-linux`  

2). Update Repositories  
`yum check-update`

3). Install  
`yum install flash-plugin nspluginwrapper alsa-plugins-pulseaudio libcurl`

Reference:  
* [Install flash on fedora](http://www.if-not-true-then-false.com/2010/install-adobe-flash-player-10-on-fedora-centos-red-hat-rhel/)  


### 2. Play music and movies  
1). add yum repository  
`su -c 'yum localinstall --nogpgcheck http://mirrors.163.com/rpmfusion/free/fedora/rpmfusion-free-release-stable.noarch.rpm http://mirrors.163.com/rpmfusion/nonfree/fedora/rpmfusion-nonfree-release-stable.noarch.rpm'`

2). install mp3 and other audio support  
`su -c 'yum install gstreamer-plugins-good gstreamer-plugins-bad gstreamer-plugins-ugly libtunepimp-extras-freeworld xine-lib-extras-freeworld'`

3). install video support  
`su -c 'yum install ffmpeg ffmpeg-libs gstreamer-ffmpeg libmatroska xvidcore'`

question:  
	Appear erro message as below when install:   
	`GPG key retrieval failed: [Errno 14] Could not open/read file:///etc/pki/rpm-gpg/RPM-GPG-KEY-rpmfusion-free-fedora-18-x86_64`  
	Check file  
	`ls /etc/pki/rpm-gpg/RPM-GPG-KEY-*`  
	Can not found /etc/pki/rpm-gpg/RPM-GPG-KEY-rpmfusion-free-fedora-18-x86_64  

Solution:  
	`su -c 'yum localinstall --nogpgcheck http://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-18.noarch.rpm http://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-18.noarch.rpm'`


Reference:  
* [Fedora 17 无法播放mp3 rmvb](http://www.linuxidc.com/Linux/2012-06/62112.htm)  
* [GPG key retrieval failed](https://ask.fedoraproject.org/question/7439/gpg-key-retrieval-failed/)

**Update:**  
Install gnome-mplayer-common rpm

### 3. How to create and use Live USB 
Reference link: <http://fedoraproject.org/wiki/How_to_create_and_use_Live_USB>
