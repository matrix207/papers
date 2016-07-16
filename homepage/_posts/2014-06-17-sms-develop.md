---
layout: post
title: "sms develop"
description: ""
category: program
tags: [sms]
---

#### Introduce
1. Different between RS232 and USB interface  
    For RS232, use serial operation directory, not need driver.   
    For USB, should intall driver.

2. OS support  
    Both windows and Linux will be supported, if the device support RS232, and 
    most of it support AT command, customer develop application with AT mostly.

#### Installation
1. Minicom

Ref:  
* <http://www.thegeekstuff.com/2013/05/modem-at-command/>  
* <http://www.cyberciti.biz/tips/connect-soekris-single-board-computer-using-minicom.html>  

2. Gnokii

#### Issue
minicom, not response for input  
Serial port setup -->Hardware Flow Contorl : NO  
Local on/off  

<http://blog.sina.com.cn/s/blog_5d0e8d0d01015svy.html>

#### AT
* get SIM ID
    
    at+ccid

* get Chip Model
    
    ati3

    at+cgmr

* get signal strength
  normal : 16~31,
  SMS err: <16 or >31(display 99)

    at+csq

* get baud rate

    at+ipr?

* set baud rate (after set, need to re-connect)

    at+ipr=115200

* save configuration

    at&w

* set SMS format
    
    at+cmgf=1

* send SMS, success if return "+CMGS: "
    
    at+cmgs=137******
    > Hi, this is a SMS test

* read SMS, 4 is the position of SMS

    at+cmgr=4

* get number of SMS center

    at+csca?

* calling

    atd137********;

* hand up
    
    ath

* check state of SMS module

    at+cfun?

* reboot SMS module
    
    at+cfun=0
    OK
    at+cfun=1
    OK

* get IMEI code

    at+wmsn or at+cgsn

* Full view

        at+ccid
        +CCID: "898600p1011130234927"

        OK
        ati3
        657e09gg.Q24PL002 1961548 103107 17:56

        OK
        at+cgmr
        657e09gg.Q24PL002 1961548 103107 17:56

        OK
        at+csq
        +CSQ: 25,0

        OK
        at+ipr?
        +IPR: 115200

        OK
        at+ipr=9600
        OK

        at
        OK
        at&w
        OK

        at+csq
        +CSQ: 25,99

        OK
        at+cmgf=1
        OK
        at+cmgs=13707070909
        > Hi, this is SMS test.
        +CMGS: 201

        OK


Ref <http://www.sendsms.cn/download/js1.pdf>

#### Features
1. auto detect device by baut-rate 
2. simple interface
    1). send message
    2). recv message

#### FAQ
1. Fail to find device
2. Fail to send message
   there are many reasons to cause this, list the main reasons as below:  
   1). device error
   1). baut-rate error
   1). signal too weak

#### Reference
* <http://bbs.chinaunix.net/thread-1793815-1-1.html>
* <http://blog.csdn.net/lyserver/article/details/3007090>
* <http://blog.chinaunix.net/uid-27717657-id-3378194.html>
* <http://www.eeworm.com/dl/693/280937.html>
* <http://download.csdn.net/detail/zhaori/7511175>
