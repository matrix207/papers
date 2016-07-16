---
layout: post
title: "Visual studio project clean"
description: "clean visual studio project"
category: windows
tags: [batch]
---

### Clean visual studio project###

	@ECHO off
	setlocal enabledelayedexpansion
	::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::
	:: Clean Visual studio project
	:: Dennis
	:: 2012-12-14
	::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

	:_LOOP
	cls
	set "prj_path="
	set /p prj_path=Drag project folder here:
	if !prj_path!. EQU . goto _EXIT
	cd !prj_path!

	del *.suo Thumbs.db /f /s /a:h 2>nul
	del *.ncb *.user /f /s 2>nul

	set /a del_type=1
	set /p del_type=Option 1 delete files,2 remove folder(default 1):
	if %del_type% GEQ 1 (
		if %del_type% LEQ 2 goto _DEL_TYPE_%del_type%
	)
	echo Please check your choise, only 1 and 2 is available.
	pause
	goto _LOOP

	:_DEL_TYPE_1
	del *.aps *.plg *.bsc *.hpj *.clw *.map *.exp *.cbp *.mdp *.ilk /f /s 2>nul
	del *.sbr *.res *.obj *.pch *.pdb *.idb *.tmp *.opt mt.dep /f /s 2>nul
	del *.manifest BuildLog.htm /f /s 2>nul
	pause
	goto _LOOP

	:_DEL_TYPE_2
	::使用*号做分隔符,解决路径中出现空格导致无法获取完整路径问题 
	@for /f "delims=*" %%i in ('dir /s /b /ad') do (
		if /i "%%~ni"=="debug" rmdir /s /q "%%i"
		if /i "%%~ni"=="release" rmdir /s /q "%%i"
	)

	pause
	goto _LOOP

	:_EXIT
	pause&exit /B 0

