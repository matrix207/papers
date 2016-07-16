---
layout: post
title: "create flowchart by graphviz"
description: ""
category: tools
tags: [graphviz, dot, flowchart]
---

### Fedora  

1.  `yum install graphviz`  

2.  write test.dot as below:  

		graph example1 {
		Server1 -- Server2
		Server2 -- Server3
		Server3 -- Server1
		}

3.  `dot -Tpng test.dot -o test.png`  
	`dot -Tsvg test.dot -o test.svg`

4. `rsvg-convert -f pdf -o test.pdf test.svg`  convert svg to pdf

### Reference
+ [使用graphviz绘制流程图](http://icodeit.org/2012/01/使用graphviz绘制流程图/)
+ <http://www.oschina.net/question/129540_79958>
+ <http://www.graphviz.org/Gallery.php>
+ <http://www.jishurensheng.com/article-hjo8380-8433934.html>
+ <http://www.fxysw.com/thread-1633-1-1.html> 
+ <http://www.wanglianghome.org/blog/2006/05/graphviz-introduction.html>
+ <http://superuser.com/questions/381125/how-do-i-convert-an-svg-to-a-pdf-on-linux>
+ <http://emacser.com/emacs_graphviz_ds.htm>
+ <http://tubocurarine.is-programmer.com/posts/27164.html>
+ <http://forum.ubuntu.org.cn/viewtopic.php?f=63&t=374855>
+ <http://www.graphviz.org/doc/libguide/libguide.pdf>
+ [linux源代码分析和阅读工具比较](http://m.oschina.net/blog/70050)
+ [看开源代码利器—用Graphviz + CodeViz生成C/C++函数调用图](http://blog.csdn.net/lanxuezaipiao/article/details/16991731)
+ [分析函数调用关系图(call graph)的几种方法](http://blog.csdn.net/Solstice/article/details/488865)
* [用CodeViz绘制函数调用关系图 call graph](http://blog.csdn.net/solstice/article/details/486788)
