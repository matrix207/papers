---
layout: post
title: "Send mail use blat"
description: "use blat to send mail in command line"
category: windows
tags: [batch]
---

#### What is blat?####  
Blat is a Windows (32 & 64 bit) command line utility that sends eMail using SMTP or post to usenet using NNTP.  

Home: [www.blat.net](www.blat.net)  
syntax: [www.blat.net/syntax/syntax.html](www.blat.net/syntax/syntax.html)  
ref: [how to send emails using blat](www.cybergav.in/2010/10/28/how-to-send-emails-using-blat/)  

#### Installation####  
`blat -installSMTP smtp.gmail.com abc@gmail.com -u username -pw yourpasswd`

#### Send mail####  
`blat -s "blat mail" -to test@gmail.com -f abc@gmail.com -body"test body"`

#### Send email with body from a file####  
`blat test.txt -s "blat mail" -to test@gmail.com -f abc@gmail.com`

#### specify charset of body file####  
`blat utf8.txt -s "blat mail" -to test@gmail.com -f abc@gmail.com -charset GBK`

#### Send attachment####  
`blat -s "blat mail" -to test@gmail.com -f abc@gmail.com -body"test attach" -attach 1.txt`

