---
layout: post
title: "Batch skill"
description: "Batch skill"
category: windows
tags: [batch]
---

### Batch skills###  

1.__获取当前日期时间__  
	set date_time=%date:~,10%_%time:~,2%_%time:~3,2%_%time:~6,2%

2.__获取当前路径__  
	set pwd=%CD%

3.__延时__  

	`ping -n 11 127.0.0.1 >nul`  

	ping 本机 11 次，可用于批处理延时 10 秒。命令中的>nul 为屏蔽输出。  
	_简短式可以写成:_  
	`ping -n 11 127.1 >nul`

4.__计算运行时间__  

		@ECHO off&setlocal enabledelayedexpansion
		CALL :TIME_START
		REM 修改这里的 ping 为你需要写的代码
		ping 127.1 -n 2 >nul
		CALL :TIME_END
		PAUSE&exit /B 0
		:TIME_START
		SET /a time_s=%time:~6,2%
		SET /a time_m=%time:~3,2%
		@ECHO Start  time: %time%
		GOTO :EOF
		:TIME_END
		@ECHO End  time: %time%
		SET /a diff_m=%time:~3,2%-%time_m%
		SET /a diff_s=%time:~6,2%-%time_s%
		SET /a total="%diff_m%"*60+"%diff_s%"
		@ECHO Elapsed time: %total% sec
		GOTO :EOF

	_运行效果:_  

		D:\work\project\tmp>test.bat
		Start  time: 13:52:00.43
		End  time: 13:52:01.48
		Elapsed time: 1 sec
		请按任意键继续. . .

5.__罗列当前目录所有 txt 文件__  

		@echo off
		for %%i in (*.txt) do echo "%%i"
		pause

6.__罗列当前和子目录所有 cfg 文件__  

	for /r 参数 遍历搜索  
	格式：`FOR /R [[drive:]path] %%variable IN (set) DO command [command-parameters]`  

	举例:  
	命令:(注意，该命令在 dos 窗口运行)  
		`D:\work\project\tmp\CI>for /r %i in (*.cfg) do @echo %i`  

	输出:  

		D:\work\project\tmp\CI\ci_cfg\ci_scm_trunk.cfg
		D:\work\project\tmp\CI\ci_cfg\ci_ubackup.cfg
		D:\work\project\tmp\CI\zd15_lcd\setup\Release\apprun.cfg
		D:\work\project\tmp\CI\zd15_lcd\setup\Release\service.cfg
		D:\work\project\tmp\CI\zd15_lcd\src\service\apprun.cfg
		D:\work\project\tmp\CI\zd15_lcd\src\service\service.cfg  

	方法二:(注意，该命令在批处理文件中运行)  
		`for /f %%i in ('dir /s /b /a-d *.cfg') do @ECHO %%i`  
	
	解决路径中出现空格  
		`for /f “delims=*”%%i in ('dir /s /b /a-d *.cfg') do @ECHO %%`  


