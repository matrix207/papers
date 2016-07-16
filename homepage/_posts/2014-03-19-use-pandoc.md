---
layout: post
title: "use pandoc"
description: ""
category: tools
tags: [write,pandoc]
---

### 1.introduce
  Pandoc is a swiss-army knife for coverting files from one markup format into 
another, it can convert documents in markdown to HTML or pdf. To see more, knock 
this [link](http://johnmacfarlane.net/pandoc/)

### 2. use 

install missing packages on fedora:

	su -c 'yum install texlive-titlesec'
	su -c 'yum install texlive-titling'
	su -c 'yum install texlive-lastpage'

generate pdf file:

	pandoc test.md -o test.pdf

using latex to generate pdf file:

	pandoc --latex-engine=xelatex  test.md -o test.pdf

generate pdf file by specify template:

	pandoc --latex-engine=xelatex --template=/home/dennis/template.latex test.md -o test.pdf

### 3.reference
+ [pandoc source](https://github.com/jgm/pandoc)
+ [利用Pandoc将markdown文件转化为pdf](http://blog.sina.com.cn/s/blog_5ee56d450101dah2.html)
+ [Markdown写作进阶：Pandoc入门浅谈](http://www.yangzhiping.com/tech/pandoc.html)
