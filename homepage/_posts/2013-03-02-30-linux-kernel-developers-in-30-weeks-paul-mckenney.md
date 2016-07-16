---
layout: post
title: "30 Linux Kernel Developers in 30 Weeks: Paul McKenney"
description: ""
category: english
tags: [Translation]
---

原文： [30-linux-kernel-developers-in-30-weeks-paul-mckenney](https://www.linux.com/news/special-feature/linux-developers/692065-30-linux-kernel-developers-in-30-weeks-paul-mckenney)


> Today we celebrate the completion of our 30 Linux Kernel Developers in 30 Weeks series. We've talked to 30 of the world's best developers over the last eight months in an effort to learn more about how the world's largest collaborative development project works. We've also learned a lot of details about what makes these amazing people tick and what advice they have for people who want to get involved.

今天可以庆祝30周30位Linux内核开发者系列文章顺产了。我们花了8个月时间采访了这个星球上最好的30位开发者,很好地了解了最大协作开发项目是如何运作的。同时也知道了是什么让这些牛人牛起来，以及他们对想入门的内核开发者的建议。

> Today we talk to Paul McKenney, who says his kernel development addicition is funded by IBM. He explains why he uses Vi and how Alexey Kuznetsov prompted Paul's first experience with Linux - and not in the way you might think.

今天采访了Paul Mckenney，IBM使他走上了内核开发的道路。他将回答为何使用Vi，以及Alexey Kuznetsov爆料Paul第一次使用Linux的经历-出乎你的意料。

Enjoy the last post in this series and stay tuned: We have a new series in the works that we hope to debut in February. I also want to give a big thanks to everyone who participated in this series.

尽情享受这一系列的最后一篇文章，也敬请关注:即将在2月份发布的新的系列作品。感谢所有参与本系列的人。

> Name

姓名

> Paul E. McKenney, IRC nickname paulmck

Paul E. McKenney, IRC 昵称 paulmck

> What role do you play in the community and/or what subsystem(s) do you work on?

你在社区中扮演什么角色，或者负责哪个子系统的开发？

> Read-copy update (RCU) is my baby; though, I do occasionally get out into other parts of the kernel and into a few user-mode projects, including userspace RCU and "Is Parallel Programming Hard, And, If So, What Can You Do About It?"

我负责维护Read-copy update (RCU锁)；但我也同时参与其他内核模块开发以及一些用户态的项目，包括用户态的RCU锁和“如何进行困难的并行编程”

> Where do you get your paycheck?

你的收入来源在哪？

> IBM Linux Technology Center funds my kernel development addiction.

IBM Linux技术中心给我的Linux开发工作提供资金。

> What part of the world do you live in? Why there?

你在哪生活?为何选择那里?

> I live in Beaverton, Oregon, USA.

我住在美国俄勒冈州的比弗顿。

> One reason is that both my wife and I grew up on Oregon.  Another reason is that Beaverton was where Sequent Computer System was located -- and they were hiring back in 1990 when it came time to move back home from the Bay Area.

原因之一是我和我的妻子都是在比佛顿长大。另外一个是因为Sequent公司在俄勒冈州 -- 他们在1990年从海湾搬回来后就开始招聘。

> What are your favorite productivity tools for software development? What do you run on your desktop?

你最喜欢的软件开发工具是什么？你通常使用什么？

> The usual productivity tools: git, cscope, vi, bash, awk, and the rare bout with python. Why vi? Well, the shared system I was using 30 years ago could support seven or eight concurrent vi sessions, but only one emacs session. In that environment, therefore, use of emacs was socially irresponsible.

常用的工具有: git，cscope，vi，bash，awk，以及python。为何用vi？在30年前我使用的共享系统能支持7或8个vi会话，但只支持1个emacs会话。在这种情况下，不合适使用emacs。

> I run Ubuntu with Unity2D at the moment.

我现在使用的是Unity2D的Ubuntu桌面系统

> How did you get involved in Linux kernel development?

你是如何参与Linux内核开发的？

> The first time was in 1997, back when I was working on DYNIX. I got an email from someone with a .ru email address asking for a machine-readable copy of an old paper of mine. I sent along a postscript of the paper and asked what he was using it for. He replied that he was working on this kernel named "Linux." Although I had heard of Linux, it would be some years before I learned the significance of the name "Alexey Kuznetsov."

第一次是在1997年，当时我还在DYNIX工作。我收到一封来自地址为.ru的邮件，想要我的一篇旧文章的电子版副本。我把文章的postscript形式版本发给他们了，并询问了用途。他回复说他为"Linux"内核工作。在我知道"Alexey Kuznetsov"这个名字好几年前，我就听说过Linux。

> The second time was in 2000, when I joined the IBM Linux Technology Center.

第二次是在2000年，我加入了IBM Linux技术中心。

> What keeps you interested in it?

是什么让你保持对Linux内核开发工作的兴趣？

> The constant challenge of keeping up with the what people are using the Linux kernel for. The need for SMP scalability, real-time response, small memory footprint, and energy efficiency (to say nothing of the reliability required to support millions of devices) has resulted in a long series of very interesting problems to solve.

对追赶上Linux内核使用者的挑战与使用Linux内核的人挑战。SMP扩展，实时响应，内存最小占用，以及高效资源使用(支持百万级别设备的可靠性)，这些都是一系列要解决的有趣问题。

> What's the most amused you've ever been by the collaborative development process (flame war, silly code submission, amazing accomplishment)?

在协作开发过程中你觉得的最消遣的是什么(嘴仗，愚蠢的代码提交，惊人的成就)。

> I am often amazed when a single technical solution addresses problems that appear to have absolutely nothing to do with each other. The first time was CONFIG_NO_HZ being required by battery-powered devices on the one hand and by mainframes on the other: The smallest of the small and the biggest of the big. Later I was surprised by how the -rt patchset was so effective at finding SMP bugs. More recently, a patch I am working on for the HPC and real-time communities might well also turn out to be useful for energy efficiency.

我常常感到惊讶，一个简单的技术解决方案在没有干扰到其他模块的情况下能很好的解决问题。第一次是CONFIG_NO_HZ同时用于电池供电设备和主机:最小中最小和最大中的最大。后面是-rt补丁包在查找SMP问题的高效应用。最近，我正在做的高性能计算(HPC)和实时通信也许对能源效率有帮助。

> What's your advice for developers who want to get involved?

你对想入门的开发者有什么建议？

> Read all the great advice from the other 29 kernel developers. I cannot think of anything to say that they have not already said. :)

参考其他29位Linux开发者的建议吧。我能想到的建议，他们都说了。

> What do you listen to when you code?

你都听些什么在编码的时候？

> I listen to contemporary music. If it is more than three or four centuries old, I have a hard time relating to it.

我听现代音乐。如果是超过三、四百年的音乐，有个固定的时期限制。

> What mailing list or IRC channel will people find you hanging out at? What conference(s)?

哪个邮件列表或IRC频道可以找到你？会议呢？

> LKML and linux-rt-users for email lists, and #linux-rt on IRC.  That said, I cannot say that I really keep up with any of them.

LKML和Linux-rt用户邮件列表，IRC频道是#linux-rt。保不定在哪个。
