---
layout: post
title: "30 Linux Kernel Developers in 30 Weeks: Sarah Sharp"
description: ""
category: english
tags: [Translation]
---

原文 [30 Linux Kernel Developers in 30 Weeks: Sarah Sharp](https://www.linux.com/news/special-feature/linux-developers/593966-30-linux-kernel-developers-in-30-weeks-sarah-sharp)

> This is the third profile in our 30-week series that features a different Linux kernel developer each week. Last week we featured Thomas Gleixner, after beginning the series with Linus Torvalds. The profiles we publish throughout the rest of 2012 will help illustrate how these developers do their work, providing important insight on how to work with them and what makes them tick.

这是30周30位Linux内核开发者系列的第三期。上周我们在首期的Linus Torvalds专访后介绍了Thomas Gleixner。在2012年余下时间中发表的这些文章将告诉大家他们是如何工作的，怎样与他们协同工作以及他们成功的原因。
 
> Name

姓名

> Sarah Sharp

Sarah Sharp

> What role do you play in the community and/or what subsystem(s) do you work on?

你最社区中扮演什么角色或者负责哪个子系统的开发？

> I'm the Linux kernel xHCI driver maintainer. I own Linux USB 3.0 support, and I send my patches up to Greg Kroah-Hartman, who is the USB subsystem maintainer.

我是Linux内核模块xHCI驱动的维护者。我负责Linux USB 3.0的开发支持，我会把补丁包提交给USB子系统维护者Grep Kroah-Hartman。

> Where do you get your paycheck?

你的收入来源？

> I work in Intel's Open Source Technology Center, along with a bunch of other cool Linux kernel developers.

我在英特尔开源技术中心和一群很酷的Linux内核开发者一起工作，

> What part of the world do you live in? Why there?

你在哪里生活？为什么？

> I live in Portland, Oregon. There's a reason why we say, "Keep Portland Weird". We have everything from Voodoo Donuts to mini-bike-riding "Zoo Bombers" to Powell's, which is America's largest book store. Portland is also really friendly to open source development. It's home to OSCON (Open Source Convention), and there's this great cross-pollination between Linux users and the bicycle community. The electronics maker community is pretty strong in Portland, too. Check out a Dorkbot meeting when you're in town, or hang out with cool techie women at Code N Splode.

我居住在俄勒冈州的波特兰。原因之一是“神奇的波特兰”。 波特兰是开源发展的朋友。是OSCON的故乡，同时是Linux用户群和车友相互交流的地方。电子产业在波特兰也很牛。？？？

> What are your favorite productivity tools for software development? What do you run on your desktop?

你最喜欢的软件开发工具是什么？你一般用什么工具？

> Um, I'm pretty much a vim and mutt person. Software development is all text for me. As for what I run on my desktop, it's Linux, of course. :) I run either Debian or Ubuntu on my boxes, and try to do all my work in Linux.

我比较中意vim和mutt.软件开发都是文本化的。当然我的桌面系统是Linux。不是Debian就是Ubuntu，我所有的工作都是在Linux下进行的。

> How did you get involved in Linux kernel development?

你是如何参与到Linux内核开发中的？

> My computer science professor, Bart Massey, was approached by Greg Kroah-Hartman, who was looking for a student to do a Linux USB project. Bart thought I would be a great fit, and was my faculty advisor for the "usbfs2" project. I worked on usbfs2 for Portland State University senior elective credits, and later as a paid project under the Intel Undergraduate Research Program.

我的计算机科学教授Bart Massey，和Greg Kroah-Hartman比较熟，他当时在找人做Linux USB项目。Bart认为我很合适，可以胜任“usbfs2”项目。usbfs2开始是我的波特兰州立大学高级选修课项目，后来变成英特尔大学生研究计划下的一个项目。

> It was pretty nerve-racking to send my first patchset out, but my then-boyfriend-now-husband encouraged me to actually hit send. The Linux USB community was a pretty good place to start doing kernel development, and the people on the mailing list were patient about answering my newbie questions.

我第一次提交补丁包的时候非常的紧张，但我的男友-现在是丈夫鼓励我发送出。Linux USB社区是一个开始内核开发的好地方，邮件列表上的人都很耐心地回答我这个信守的问题。

> A couple months before I graduated, I proposed a talk on my usbfs2 project for OSCON 2007. Kristen Accardi was on the OSCON selection committee, and remembered me from some Portland Linux gatherings I had been to. She knew that Intel's Open Source Technology Center was looking for a Linux USB developer, so she got me an interview with OTC. I've been working for OTC for the past five years now, continuing my work on the Linux USB subsystem.

在毕业前几个月，我在2007年度的OSCON上做了usbfs2的演讲。OSCON选举委员会的Kristen Accardi看到我来自波特兰Linux群。她知道英特尔的开源技术中心正在招聘Linux USB开发员，所以她给我一个面试机会。我研究在英特尔开源技术中心做了5年的Linux USB子系统开发。

> I wouldn't be a Linux kernel developer today without the contacts I created by networking with other developers at conferences and tech events.

如果没有集会和技术论坛的其他开发者的交流帮助，我不可能成为一个Linux内核开发者。

> What keeps you interested in it?

是什么让你兴趣不减？

> The people keep me coming back. I really love learning and discussing new ideas with the Linux kernel community, and my Intel co-workers. Sure, there's some heated discussions every once in a while, but most developers are friendly enough to take the time and answer any questions I have.

我喜欢和Linux内核论坛以及我的英特尔同事，一起学习、讨论新的想法。确实，时常会有一些激烈的讨论，但大多数开发者都是很友好的花时间回答我的任何问题。

> What's the most amused you've ever been by the collaborative development process (flame war, silly code submission, amazing accomplishment)?

在协作开发过程中你觉得好玩的是什么？（嘴仗，愚蠢代码的提交，惊人的成就）？

> I've certainly amused Greg KH in the past with my pull request descriptions. Here's an excerpt from a reply he sent me:

我觉得最搞的是Greg KH对我非常愚蠢的请求的回应。这里是回信的片段：

> Date: Thu, 26 May 2011 00:04:50 -0700
> From: Greg KH 
> To: Sarah Sharp 
> Cc: linux-usb@vger.kernel.org
> Subject: Re: \[RFC 0/3\] xhci: Remove useless debugging
> 
> On Wed, May 25, 2011 at 04:28:51PM -0700, Sarah Sharp wrote:
>             The xHCI driver is what, two years old now? It's high time it put on
>             its big girl pants and stop crapping out useless debugging information.
> 
>     you now owe me a wet-wipe to clean my laptop up from the coffee i just
>     snorted out my nose all over it.

2011年5月25日，星期三，早上04:28:51，Sarah Sharp写道：
			xHCI驱动两年了？是时候

你现在欠我一条湿纸巾

> What's your advice for developers who want to get involved?

对想进入该领域的开发者你有什么建议？

> Find a medium-sized project, in a part of the Linux kernel community that has a responsive mailing list. Don't waste your time on a bunch of spelling fix patches. A couple bug fix patches should be enough to get you used to the patch submission process, but at some point you need to move on and start making useful, bigger, contributions to the kernel.

找一个中等的项目，在linux内核社区中有很好的邮件列表。别浪费时间在拼写错误的补丁包上。

> Find a mentor. It doesn't have to be someone who is a Linux subsystem maintainer. It should be someone who knows git basics, can review your code, and help you set up a mail client so you can send patches. Team up with a friend to do joint code review and try to figure out a Linux kernel subsystem together.

找一个有经验的人。不一定非要是Linux子系统的维护者。只要这个人知道基本的git用法，可以review你的代码，以及可以帮助你配置提交补丁包的邮件客户端。和你的朋友进行互相code review的关系，并且一起尝试进行Linux内核子系统的学习。

> What do you listen to when you code?

在编码的时候你都听些什么？

> I can't concentrate on coding when I listen to vocals, so I tend to lean towards electronica, break beats, or classical movie sound tracks. I usually listen to Daft Punk, Justice, Hans Zimmer, and Klaus Badelt.

太吵闹的音乐我是无法集中精神编码的，所以我倾向于电子音乐，慢节拍，古典电影的插曲。我常听Daft Punk, Justice, Hans Zimmer, and Klaus Badelt。

> What mailing list or IRC channel will people find you hanging out at? What conference(s)?

在哪些邮件列表或IRC频道可以找到你？会议呢？

> I'm on the linux-usb@vger.kernel.org mailing list. As for conferences, I go to OSCON, Open Source Bridge, LinuxCon America, Linux Plumbers Conf, Linux Kernel Summit, and Linux Conf Australia. This year, I'll also be at AdaCamp D.C., representing women in open source.

我在linux-usb@vger.kernel.org邮件列表。会议的话，我会参加OSCON, 开源Bridge，美国Linux会议，Linux Plumbers Conf, Linux内核峰会以及澳大利亚Linux会议。今年我将代表开源中的女性参加华盛顿的AdaCamp。
