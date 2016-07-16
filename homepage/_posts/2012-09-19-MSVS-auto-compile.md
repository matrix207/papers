---
layout: post
title: "MS Visual Studio auto compile"
description: "MS Visual Studio auto compile"
category: windows
tags: []
---

### 文件列表
* 主程序 `main.bat`
* 配置文件 `project.cfg`
* 编译器环境设置 `vsvars.bat`


### 操作步骤
* 配置 `project.cfg` 文件
* 执行 `main.bat`


### 文件源码
__project.cfg__

	compiler:2008
	pri_name:SampleProject
	prj_dir:../SampleProject

__vsvars.bat__

	@ECHO off

	SET /a retval=0

	GOTO CASE_%1
	:CASE_2005
		IF NOT "%VS80COMNTOOLS%"=="" (
			CALL "%VS80COMNTOOLS%"vsvars32.bat
			SET /a retval=1
		)
		GOTO END_SWITCH
	:CASE_2008
		IF NOT "%VS90COMNTOOLS%"=="" (
			CALL "%VS90COMNTOOLS%"vsvars32.bat
			SET /a retval=1
		)
		GOTO END_SWITCH
	:CASE_2010
		IF NOT "%VS100COMNTOOLS%"=="" (
			CALL "%VS100COMNTOOLS%"vsvars32.bat
			SET /a retval=1
		)
		GOTO END_SWITCH
	:END_SWITCH

	EXIT /B %retval%

__main.bat__

	:: Function: Build/Clean Visual Studio Project
	:: Author:   Dennis
	:: Date:     2012-09-18

	@ECHO off&setlocal enabledelayedexpansion 

	mode con lines=30 cols=100
	title Build/Clean Visual Studio project

	REM +++++++++++++++++++++++++++++++++
	REM Get project name from config file
	REM +++++++++++++++++++++++++++++++++
	SET prj_name=
	SET compiler=
	SET prj_dir=

	FOR /f "tokens=1* delims=:" %%a IN (project.cfg) DO (
		IF "%%a"=="compiler" SET compiler=%%b
		IF "%%a"=="pri_name" SET prj_name=%%b
		IF "%%a"=="prj_dir" SET prj_dir=%%b
	)

	IF "%compiler%"=="" (
		ECHO "Config project compiler first."
		GOTO End
	)

	IF "%prj_name%"=="" (
		ECHO "Config project name first."
		GOTO End
	)

	IF "%prj_dir%"=="" (
		ECHO "Config project diretory first."
		GOTO End
	)

	REM +++++++++++++++++++++++++++++++++
	REM Set compile environment
	REM +++++++++++++++++++++++++++++++++
	:Vars
	CALL vsvars.bat %compiler%
	IF %ERRORLEVEL% LEQ 0 (
		ECHO "Error code: %ERRORLEVEL%"
		ECHO "Failed to SET VS %compiler% complile enviroment."
		ECHO "Please check and try again."
		GOTO End
	)

	REM +++++++++++++++++++++++++++++++++
	REM Main process
	REM +++++++++++++++++++++++++++++++++
	:Main
	CLS
	ECHO.
	ECHO. %prj_name% project manager
	ECHO.
	ECHO. Porject type:
	ECHO. 	1.Release
	ECHO. 	2.Debug
	ECHO.
	ECHO. Opereate type:
	ECHO. 	1.Build
	ECHO. 	2.Clean
	ECHO. __________________________________________________
	ECHO.

	SET choice=
	SET /p choice= Please select project type(EXIT IF enter)：
	IF "%choice%"=="1" ( SET prj_type="Release" & GOTO Next_Choise )
	IF "%choice%"=="2" ( SET prj_type="Debug" & GOTO Next_Choise )
	IF DEFINED choice GOTO Main
	EXIT

	:Next_Choise
	ECHO Your selected is %prj_type%
	SET /p choice= Please select operate type(EXIT IF enter)：
	IF "%choice%"=="1" GOTO Case_Operate_%choice%
	IF "%choice%"=="2" GOTO Case_Operate_%choice%
	IF DEFINED choice GOTO Main
	EXIT

	:Case_Operate_1
		devenv.com %prj_dir%\%prj_name%.vcproj /build %prj_type%
		GOTO End_Switch
	:Case_Operate_2
		devenv.com %prj_dir%\%prj_name%.vcproj /clean %prj_type%
		GOTO End_Switch
	:End_Switch
		PAUSE
		GOTO Main


	:End
	PAUSE

