---
layout: post
title: "install skype on fedora18 x86_64"
description: ""
category: linux
tags: [skype]
---

1. 保存以下脚本为 skype-on-fedora18.sh

		#!/bin/sh
		################################################################################
		# Install skype on fedora 18 x86_64
		# Reference: http://www.if-not-true-then-false.com/2012/install-skype-on-fedora-centos-red-hat-rhel-scientific-linux-sl/ 
		################################################################################

		yum install -y alsa-lib.i686 fontconfig.i686 freetype.i686 glib2.i686 libSM.i686 libXScrnSaver.i686 libXi.i686 libXrandr.i686 libXrender.i686 libXv.i686 libstdc++.i686 pulseaudio-libs.i686 qt.i686 qt-x11.i686 zlib.i686

		yum install -y qtwebkit.i686

		cd /tmp
		 
		## Skype 4.1 Dynamic for Fedora ##
		wget --trust-server-names http://www.skype.com/go/getskype-linux-dynamic

		mkdir /opt/skype
		 
		## Extract Skype 4.1 on Fedora ##
		tar xvf skype-4.1* -C /opt/skype --strip-components=1

		ln -s /opt/skype/skype.desktop /usr/share/applications/skype.desktop
		ln -s /opt/skype/icons/SkypeBlue_48x48.png /usr/share/icons/skype.png
		ln -s /opt/skype/icons/SkypeBlue_48x48.png /usr/share/pixmaps/skype.png
		 
		touch /usr/bin/skype
		chmod 755 /usr/bin/skype

		cat <<EOF >> /usr/bin/skype
		#!/bin/sh
		export SKYPE_HOME="/opt/skype"
		 
		\$SKYPE_HOME/skype --resources=\$SKYPE_HOME \$*
		EOF

2. 增加可执行权限 chmod +x skype-on-fedora18.sh

3. 执行脚本 ./skype-on-fedora18.sh

4. 安装完毕后运行 skype


Reference :

[Install Skype 4.1/4.0 on Fedora 18/17, CentOS/RHEL/SL 6.3](http://www.if-not-true-then-false.com/2012/install-skype-on-fedora-centos-red-hat-rhel-scientific-linux-sl/ )
