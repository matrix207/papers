---
layout: post
title: "maxima tutorial"
description: ""
category: math 
tags: [maxima]
---

1. _安装_ `yum install maxima`
2. _运行_ `maxima`
3. _例子_  

		[root@localhost ~]# maxima --version
		Maxima 5.29.1
		[root@localhost ~]# maxima
		Maxima 5.29.1 http://maxima.sourceforge.net
		using Lisp SBCL 1.1.2-1.fc18
		Distributed under the GNU Public License. See the file COPYING.
		Dedicated to the memory of William Schelter.
		The function bug_report() provides bug reporting information.
		(%i1) 2+3;
		(%o1)                                  5
		(%i2) 1/3;
		                                       1
		(%o2)                                  -
		                                       3
		(%i3) float(1/3);
		(%o3)                          .3333333333333333
		(%i4) sum(i^2, i, 1, 3);
		(%o4)                                 14
		(%i5) ? if

		 -- Special operator: if
			 Represents conditional evaluation.  Various forms of `if'
			 expressions are recognized.

			 `if <cond_1> then <expr_1> else <expr_0>' evaluates to <expr_1> if
			 <cond_1> evaluates to `true', otherwise the expression evaluates
			 to <expr_0>.

			 The command `if <cond_1> then <expr_1> elseif <cond_2> then
			 <expr_2> elseif ... else <expr_0>' evaluates to <expr_k> if
			 <cond_k> is `true' and all preceding conditions are `false'.  If
			 none of the conditions are `true', the expression evaluates to
			 `expr_0'.

			 A trailing `else false' is assumed if `else' is missing.  That is,
			 the command `if <cond_1> then <expr_1>' is equivalent to `if
			 <cond_1> then <expr_1> else false', and the command `if <cond_1>
			 then <expr_1> elseif ... elseif <cond_n> then <expr_n>' is
			 equivalent to `if <cond_1> then <expr_1> elseif ... elseif
			 <cond_n> then <expr_n> else false'.

			git/ The alternatives <expr_0>, ..., <expr_n> may be any Maxima
			 expressions, including nested `if' expressions.  The alternatives
			 are neither simplified nor evaluated unless the corresponding
			 condition is `true'.

			 The conditions <cond_1>, ..., <cond_git/n> are expressions which
			 potentially or actually evaluate to `true' or `false'.  When a
			 condition does not actually evaluate to `true' or `false', the
			 behavior of `if' is governed by the global flag `prederror'.  When
			 `prederror' is `true', it is an error if any evaluated condition
			 does not evaluate to `true' or `false'.  Otherwise, conditions
			 which do not evaluate to `true' or `false' are accepted, and the
			 result is a conditional expression.

			 Among other elements, conditions may comprise relational and
			 logical operators as follows.

				  Operation            Symbol      Type

				  less than            <           relational infix
				  less than            <=
					or equal to                    relational infix
				  equality (syntactic) =           relational infix
				  negation of =        #           relational infix
				  equality (value)     equal       relational function
				  negation of equal    notequal    relational function
				  greater than         >=
					or equal to                    relational infix
				  greater than         >           relational infix
				  and                  and         logical infix
				  or                   or          logical infix
				  not                  not         logical prefix


		  There are also some inexact matches for `if'.
		  Try `?? if' to see them.git/

		(%o5)                                true
		(%i6) 
		(%i6) quit ();
		[root@localhost ~]# 

4. _画图_  

	`plot2d(x^3+2*x^2-3,[x,-2,2]);`

	![plot2d](/assets/image/posts/maxima-plot2d.png)

	`plot3d(sin(x)*cos(y),[x,-2,2],[y,-2,2]);`

	![plot3d](/assets/image/posts/maxima-plot3d.png)

5. _编程_  
	创建一个后缀为max(可以是任何后缀)的文件,如test.max,编辑代码后保存.  
	在maxima环境中执行`batch("test.max")`

	test.max

		a:2;
		b:5;
		a^3+b^2;

	执行效果

		(%i3) batch("test.max");

		read and interpret file: /root/prj/git/euler/maxima/test.max
		(%i4)                                a : 2
		(%o4)                                  2
		(%i5)                                b : 5
		(%o5)                                  5
	                                        2    3
		(%i6)                               b  + a
		(%o6)                                 33
		(%o6)                 /root/prj/git/euler/maxima/test.max

	关于batch的更多用法，请查阅帮助手册中的章节`13.3 Functions and Variables for File Input and Output`

6. _其他范例_  
	[Project Euler第1题](http://projecteuler.net/problem=1)  

		sum(if (mod(i, 3) = 0) or (mod(i, 5) = 0) then i else 0, i, 1, 999)

	[Project Euler第2题](http://projecteuler.net/problem=1)  
	euler2.max

		s:0$
		for i:3 while fib(i)<=4*10^6 do (s:s+fib(i), i:i+2)$
		display(s);
	
	执行效果

		(%i164) batch("euler2.max");

		read and interpret file: /root/prj/euler/maxima/euler2.max
		(%i165)                              s : 0
												 6
		(%i166) for i from 3 while fib(i) <= 4 10  do (s : fib(i) + s, i : 2 + i)
		(%i167)                           display(s)
										  s = 4613732

		(%o167)                              done
		(%o167)              /root/prj/euler/maxima/euler2.max	

7. _相关语法说明_  
	`;` 代码结束符    
	`$` 多行代码  
	`,` 多个语句使用`(sentence1, sentence2)`  

8. _帮助手册_  
	浏览器查看文件`/usr/share/maxima/5.29.1/doc/html/maxima.html`

相关教程资源:  
+ [Maxima 5.28.0 Manual](http://maxima.sourceforge.net/docs/manual/en/maxima.html)
+ [Linux下的数学工具Maxima 简明教程 上](http://www.matrix67.com/blog/archives/337)
+ [Linux下的数学工具Maxima 简明教程 下](http://www.matrix67.com/blog/archives/338)
+ [The Computer Algebra Program Maxima - a Tutorial](http://maxima.sourceforge.net/docs/tutorial/en/gaertner-tutorial-revision/Contents.htm)
+ [maxima 笔记](http://tranzing.blogspot.com/2005/01/maxima.html)
+ [andrejv's blog](http://andrejv.wordpress.com/category/maxima/)
+ [一些代码](http://inside.mines.edu/fs_home/whereman/software.html)
+ [一些代码](http://inside.mines.edu/fs_home/whereman/software/painleve/macsyma/single/np_sing.max)
+ [A 10 minute tutorial for solving math problems with maxima](http://math-blog.com/2007/06/04/a-10-minute-tutorial-for-solving-math-problems-with-maxima/)
+ [Maxima快速参考手册](https://webfiles.uci.edu/huanm/www/maxima/maxima_zh.pdf)

