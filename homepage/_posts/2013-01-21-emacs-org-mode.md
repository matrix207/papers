---
layout: post
title: "Emacs org mode"
description: ""
category: tools
tags: [emacs]
---

__网络资料__

+ [org tutorials](http://orgmode.org/worg/org-tutorials/)
+ [Emacs org mode学习笔记](http://emacser.com/org-mode.htm)
+ [org-mode，最好的文档编辑利器，没有之一](http://www.cnblogs.com/holbrook/archive/2012/04/12/2444992.html#sec-3-5)
+ [Org-mode写作的几个快捷方式](http://emacser.com/org-mode-tricks.htm)
+ [组织你的意念：Emacs org mode](http://i.linuxtoy.org/docs/guide/ch32.html)
+ [神器中的神器org-mode之入门篇](http://www.cnblogs.com/qlwy/archive/2012/06/15/2551034.html)

-----

___解决导出pdf无法显示中文问题___  

+  _安装xetex_  
	`yum install texlive-xetex-bin.x86_64`

+  _安装其他缺少的包_  
	`yum install texlive-xetex-def`  
	`yum install texlive-xltxtra`  
	`yum install texlive-xecjk`  
	`yum install texlive-wrapfig`  

+  _问题_: LaTeX Error: Encoding scheme EU1 unknown.  
	解决: 使用yum安装`yum install 'tex(eu1enc.def)'` 或 `yum install texlive-euenc`，ok问题解决，可以生成中文pdf了.  
	参考: [fedora 17 ctex 中文](http://hi.baidu.com/coco_zhao/item/0ceb6ff40885ca68922af276)

___下划线转义问题___

+  _为了不要转义下划线，如`my_test`中的下划线保留，需在org文件头部加入`#+OPTIONS: ^:nil`_

__参考文章__:   
+ [配置Emacs org-mode利用latex生成pdf文件](http://www.cnblogs.com/visayafan/archive/2012/06/16/2552023.html)
+ [LaTeX Export In Emacs Org-Mode](http://tangboyun.ixiezi.com/index.php/2011/05/latex-export-in-emacs-org-mode/)
+ [Org Mode 标记语言的一些疑问](http://blog.waterlin.org/articles/emacs-org-mode-subscripter-setting.html)

-----

+ [使用Org-mode和html5slides制作幻灯片](http://www.worldhello.net/gotgithub/index.html)
> html5slides 是 Google 公司的一个开源项目，可使用 HTML5 技术制作幻灯片。
> 在线演示：
> [http://html5slides.googlecode.com/svn/trunk/template/index.html](http://html5slides.googlecode.com/svn/trunk/template/index.html)

-----
