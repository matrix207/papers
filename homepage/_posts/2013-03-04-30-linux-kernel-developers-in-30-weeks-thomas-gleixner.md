---
layout: post
title: "30 Linux Kernel Developers in 30 Weeks: Thomas Gleixner"
description: ""
category: english
tags: [Translation]
---

原文:[30 Linux Kernel Developers in 30 Weeks: Thomas Gleixner](https://www.linux.com/news/special-feature/linux-developers/591039-30-linux-kernel-developers-in-30-weeks-thomas-gleixner)

> This is the second in our 30-week series that profiles a different Linux kernel developer each week. Last week we debuted the series with Linus Torvalds. The profiles we publish throughout the rest of 2012 should help illustrate how these developers do their work, providing important insight on how to work with them and what makes them tick.

这个30周30位Linux内核开发者介绍的第二期。上周我们采访了Linus Toravalds。在2012年余下时间中发表的这些文章将告诉大家他们是如何工作的，怎样与他们协同工作以及他们成功的原因。

> Name

名字

> Thomas Gleixner, nickname: tglx

Thomas Gleixner, 昵称: tglx

> What role do you play in the community and/or what subsystem(s) do you work on?

你在社区中扮演什么觉得以及维护哪部分？

> Quite a few people consider me to be one of the Grumpy Old Men. That's related to my age and the age-related unwillingness to cope with crap.

很多人认为我是脾气暴躁的老人家。这涉及到我的年纪，但和年纪一丁点关系都没有。

> As a maintainer I'm responsible for the core infrastructure of timers, timekeeping and interrupt handling. I'm part of the x86 architecture maintainer team and I'm the maintainer and main developer of the real time preemption patch. Aside of that I have a strong affinity for mission impossible and tree wide code cleanups.

作为一个维护人员，我主要负责定时器的核心基础，时间记录器以及终端处理。我是x86架构维护组成员，实时抢占补丁的开发和维护者。除此之外，我喜欢一些高难度任务，以及内核树清理工作。

> Where do you get your paycheck?

你的收入来源？

> From my own company, which gets part of it refunded by contracts with Red Hat and others interested in my work.

来自我的公司，其中一部分是红帽子的合约，其他的是我的工作。

> What part of the world do you live in? Why there?

你在哪生活？为什么？

> Germany. It's my home, why should I live anywhere else?

德国。这里是我的家，我不可能去其他地方。

> What are your favorite productivity tools for software development?

你最喜欢的软件开发工具是什么？

> Command line tools. Don't try to involve me into Emacs vs. VI discussions and don't ask me about GUI tools :)

命令行工具。不要让我陷入Emacs与VI论战，也不要问一些GUI工具。:)

> What do you run on your desktop?

你通常使用什么工具？

> Changing flavors of Linux distributions. My desktop requirements are rather low: Manage a gazillion of terminals, run a graphical browser and occasionally fire up some unavoidable GUI applications.

修改Linux分发包的特性。我对桌面要求很低:管理一个图形终端，运行一个界面浏览器，偶尔修改一些负责的GUI应用程序。

> I'm desperately trying to avoid the new fangled app driven "desktops," which insist on knowing better than I how to manage my workflow efficiently.

我尽可能的避免在桌面中运行新的应用程序，

> How did you get involved in Linux kernel development?

你是如何进入到Linux内核开发的？

> Curiosity.

好奇心使然。

> What keeps you interested in it?

什么让你一如既往干这事？

> The fun of it. Working with smart people all over the world.

这是件有意思的工作。同时也可以和全世界最聪明的人一起工作。

> Image: LinuxCon Europe, Gleixner second from right

图片:  欧洲Linux会议，Gleixner在右边第二个

> What's the most amused you've ever been by the collaborative development process (flame war, silly code submission, amazing accomplishment)?

在协作开发过程中你觉得最好玩的是什么？（嘴仗，愚蠢代码提交，惊人的成就）？

> That's a tough question. I have my favorites in all categories, but as far as silliness, this is my favorite one:

这是一个很难回答的问题。我喜欢所有的模块，但我觉得最傻的是下面这个：

> +       d->core_internal_state__do_not_mess_with_it |= SOME_CONSTANT;

+       d->core_internal_state__do_not_mess_with_it |= SOME_CONSTANT;

> See http://www.spinics.net/lists/linux-tip-commits/msg11099.html

参考 [http://www.spinics.net/lists/linux-tip-commits/msg11099.html](http://www.spinics.net/lists/linux-tip-commits/msg11099.html)

> What's your advice for developers who want to get involved?

你对想进入该领域的开发者有什么建议？

> Find your area of interest, and start tackling problems which are affecting you.

找到你感兴趣的领域，然后解决对你有帮助的问题。

> What do you listen to when you code?

你最编码时都听些什么？

> To the thoughts drifting through my brain.

我

> What mailing list or IRC channel will people find you hanging out at? What conference(s)?

在那些邮件列表或IRC频道可以找到你？会议呢？

> Mailing lists: Mostly LKML (Linux Kernel Mailing List)
> IRC channels : My nick is unique
> Conferences  : Too many

邮件列表: 基本上是LKML（Linux内核列表）
IRC频道: 我的昵称是 unique
会议: 太多了

> Thanks to Thomas for participating in 30 Linux Kernel Developers in 30 Weeks. Next week, we talk to Sarah Sharp.

谢谢Thomas参加30周30位Linux内核开发者。下周我们将对话Sarah Sharp。
