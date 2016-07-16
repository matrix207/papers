---
layout: post
title: "Viusal studio project auto compile"
description: "Viusal studio project auto compile"
category: windows
tags: [batch]
---


### Windows Requirements ###
	microsoft visual studio 2005 or newer
	subversion 1.7.7 or newer

	add the path of command tool "svn" and "devenv" to environment path

### check out project by svn###  

`svn co project_svn_url output_path -r revision`  

__svn_co.bat__  

	@ECHO off&SETlocal enabledelayedexpansion 

	REM Useage: svn_co.bat svn_src_url out_path revision log_file

	SET func="check out project"

	SET src_url=%1
	SET out_path=%2
	SET src_rev=%3
	SET log_file=%4
	SET parameters=%src_url%

	IF NOT "%out_path%"=="" SET parameters=!parameters! %out_path%

	IF NOT "%src_rev%"=="" SET parameters=!parameters! -r %src_rev%

	SET log_file_tmp=NUL
	IF NOT "%log_file%"=="" SET log_file_tmp=%log_file%

	svn co !parameters!

	SET /a ret_code=%ERRORLEVEL%
	IF %ret_code% EQU 0 (
		@ECHO svn checkout success
	) ELSE (
		@ECHO svn checkout fail
	)


### use devenv for compile###  

`devenv solution_file /build "release|win32"`  

__compile.bat__  

	@ECHO off

	REM Useage: compile.bat solution_file

	SET sln_file=%1
	SET cfg_build="Release|Win32"

	devenv %sln_file% /build %cfg_build%

	SET /A retval=%ERRORLEVEL%
	EXIT /B %retval%

