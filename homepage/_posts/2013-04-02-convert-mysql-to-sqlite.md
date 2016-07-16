---
layout: post
title: "convert mysql to sqlite"
description: ""
category: database
tags: [database]
---

### 安装 mysql
	yum install mysql

### 安装 sqlite
	yum install sqlite-devel

### MySql常用命令

	登陆数据库
	[root@localhost ~] mysql -uroot -p123456 mysql

	显示已有数据库
	mysql> show databases;
	 
	进入数据库
	mysql> use mytest;
	 
	数据库中的表
	mysql> show tables;


### 脚本实现: mysql 转为 sqlite  
[mysql2sqlite.sh](https://gist.github.com/esperlu/943776/)

### 终端操作记录
	# 上传脚本
	[root@localhost sqlite]# sshpass -p 1234 scp mysql2sqlite.sh 192.168.1.103:/root
	[root@localhost sqlite]# ssh 172.16.50.117
	[root@localhost ~]# ls
	mysql2sqlite.sh

	# 执行转换mysql数据库到sqlite数据库,转换后的文件为database.sqlite
	[root@localhost ~]# ./mysql2sqlite.sh -u root -p123456 mysql | sqlite3 database.sqlite
	memory
	[root@localhost ~]# ls -lh
	total 632K
	-rw-r--r-- 1 root root 629K 2013-04-02 09:08 database.sqlite
	-rwxr-xr-x 1 root root 3.0K 2013-04-02 08:49 mysql2sqlite.sh

	# 打开数据库文件
	[root@localhost ~]# sqlite3 database.sqlite  
	SQLite version 3.6.17
	Enter ".help" for instructions
	Enter SQL statements terminated with a ";"
	sqlite> .database                   # 显示数据库
	seq  name             file                                                      
	---  ---------------  ----------------------------------------------------------
	0    main             /root/database.sqlite                                     
	sqlite> .tables                     # 显示表格
	columns_priv               proc                     
	db                         procs_priv               
	event                      servers                  
	func                       tables_priv              
	help_category              time_zone                
	help_keyword               time_zone_leap_second    
	help_relation              time_zone_name           
	help_topic                 time_zone_transition     
	host                       time_zone_transition_type
	ndb_binlog_index           user                     
	plugin                   
	sqlite> select * from user;         # 查询表格信息
	....                                # 此处省略输出
	sqlite> .exit                       # 退出
	[root@localhost ~]#

	# 导出其他数据库mytest, 这里出现了错误,待解决.
	[root@localhost ~]# ./mysql2sqlite.sh -u root -p123456 mytest | sqlite3 mytest.sqlite
	memory
	SQL error near line 746: near "COMMENT": syntax error
	SQL error near line 808: no such table: main.user_log
	SQL error near line 809: no such table: main.user_log
	[root@localhost ~]# ls

reference:
* [mysql2sqlite.sh](https://gist.github.com/esperlu/943776/)
* [MySQL命令](http://my.oschina.net/chunquedong/blog/25913)
* [MYSQL命令大全](http://my.oschina.net/ncu033/blog/103540)
* [Export a MySQL Database to SQLite Database](http://stackoverflow.com/questions/5164033/export-a-mysql-database-to-sqlite-database)
* [sqlite - Converter Tools](http://www.sqlite.org/cvstrac/wiki?p=ConverterTools)
* [sqlite3 命令行操作](www.cnblogs.com/lwm-1988/archive/2011/08/20/2147573.html)
* [mysqldump备份还原](www.cnblogs.com/zeroone/archive/2010/05/11/1732834.html) 
* [How-to: Convert mysql to sqlite](http://www.jbip.net/content/how-convert-mysql-sqlite)
* [mysql to sqlite database](http://blog.sina.com.cn/s/blog_a9ec688101019m8i.html)
* [How to install SQLite3 on Fedora](http://radicallyrails.blogspot.com/2011/05/how-to-install-sqlite3-on-fedora.html)
* [sqlite3-to-mysql](https://github.com/athlite/sqlite3-to-mysql/blob/master/sqlite3-to-mysql)
* [Shell scripts for converting MySQL dump into SQLite](www.stat.berkeley.edu/~statcur/Workshop2/Assignments/Baseball/instructions.html)
* [MySQL转SQLite Shell脚本](http://mifunny.info/convert-mysql-to-sqlite-script-330.html)
