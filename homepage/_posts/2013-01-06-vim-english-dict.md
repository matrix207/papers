---
layout: post
title: "vim english dict"
description: ""
category: tools
tags: [vim]
---

#### vim英文字典####  

+  _下载字典_  
	地址:[http://www.vim.org/scripts/script.php?script_id=195](http://www.vim.org/scripts/script.php?script_id=195)

+  _解压缩所需字典文件_  
	`tar xvf ~/Downloads/engspchk.tar.gz CVIMSYN/engspchk.dict`  

+  _使用vim整理成字典文件_  
	`vim CVIMSYN/engspchk.dict`  
	删除前面三行: `gg3dd`  
	删除行首GoodWord单词: `:%s/^GoodWord\t//g`  
	整理成每个单词占一行: `:%s/\t//g`  
	删除最后一行: `Gdd`  
	排序: `:sort`  

+  _拷贝字典文件到指定目录_  
	`mkdir /usr/share/vim/vim73/dict`    
	`cp CVIMSYN/engspchk.dict /usr/share/vim/vim73/dict/english.dict`  

+  _设置.vimrc文件_  
	`setlocal dictionary+=$VIMRUNTIME/dict/english.dict`

+  _英文单词自动补全快捷键_  
	Ctrl+X Ctrl+K

