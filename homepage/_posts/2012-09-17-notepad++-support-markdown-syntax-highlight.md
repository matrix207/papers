---
layout: post
title: "Notepad++ support highlight of markdown"
description: ""
category: windows
tags: [write]
---

Notepad++默认不支持Markdown格式文件`md`高亮显示.

Github上的thomsmits童鞋填补了该空缺
[https://github.com/thomsmits/markdown_npp](https://github.com/thomsmits/markdown_npp)

这里也贴一下步骤:
* 浏览器打开[userDefineLang.xml](https://github.com/thomsmits/markdown_npp/blob/master/userDefineLang.xml)
* 复制全部代码,即从`<NotepadPlus>`到`</NotepadPlus>`, 粘贴并保存为文件`userDefineLang.xml`
* 打开Notepad++设置目录, `开始` > `运行`, 输入 (或粘贴)`%APPDATA%\Notepad++`, 点击`确定`
* 复制刚才保存的`userDefineLang.xml`到Notepad++设置目录
* 重启Notepad++

就可以在Noepad++的菜单项`语言`下找到`Markdown`
