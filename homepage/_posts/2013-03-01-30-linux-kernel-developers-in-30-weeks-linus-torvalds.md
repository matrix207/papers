---
layout: post
title: "30 Linux Kernel Developers in 30 Weeks: Linus Torvalds"
description: ""
category: english
tags: [Translation]
---

原文: [30 Linux Kernel Developers in 30 Weeks: Linus Torvalds](https://www.linux.com/news/special-feature/linux-developers/587846-30-linux-kernel-developers-in-30-weeks-linus-torvalds)

> Welcome to 30 Linux Kernel Developers in 30 Weeks! This is the first in a 30-week series we're running that profiles a different Linux kernel developer each week. The Linux kernel development community is unique in many ways. The individuals that make up this community are responsible for orchestrating the world's largest collaborative development project and have a very large impact on the future of the operating system and other technologies. The profiles we publish throughout the rest of 2012 should help illustrate how these developers do their work, providing important insight on how to work with them and what makes them tick.

欢迎来到30周30位Linux内核开发者！这是介绍不同Linux内核开发者30周系列的第一个。Linux内核开发社区是独一无二的。社区中的每个开发者负责设计世上最大的协助开发项目，并且对操作系统和技术的未来发展有很大的影响。在余下的2012年中做的这些介绍，将阐述他们如何工作，如何跟他们一起工作以及什么使得他们伟大。

> We start the series with none other than Linux creator Linus Torvalds. This week Linus is in his home country of Finland to attend the Millennium Technology Prize ceremony. This week he was named a joint winner of the 2012 Millenium Technology Prize. Just before he left, he took the time to answer these profile questions.

开篇介绍的是Linus Torvalds。本周Linus在他的家乡芬兰参加千禧技术奖典礼，成为2012千禧技术奖的得主之一。在他离开前，作了我们的采访。

> What's your name?

你的名字是？

> Linus Torvalds

Linus Torvalds

> What role do you play in the community and/or what subsystem(s) do you work on?

你在社区中扮演什么角色或负责哪个子系统的开发？

> I'm the kernel top-level maintainer and don't tend to do any particular subsystems directly; although, I occasionally get directly involved with the VFS layer (and very occasionally some VM discussions).

我是内核的最高维护者，不直接参加任何子系统开发。但是偶尔会参与VFS界面(以及更偶尔地参加VM讨论)

> Where do you get your paycheck?

你的收入来源是什么？

> The Linux Foundation.

Linux基金会。

> What part of the world do you live in? Why there?

你在哪居住？为什么？

> Portland, Oregon. As to "why?" it's mostly because it's a lot calmer and more livable than Silicon Valley, where we lived for several years before moving here. The weather may not be that great, but it's a much better area for the kids to grow up in, I suspect. And we can afford a bigger house in a good school district.

俄勒冈州，波特兰。原因是这里安静，比硅谷更适合居住，在哪边生活了好几年。也许气候没有那么好，但我相信这里更适合孩子的成长。还有在这边名校的区供得起大房子。

> What are your favorite productivity tools for software development? What do you run on your desktop?

你最喜爱的软件开发工具是什么？你通常使用什么？

> I really just run a web browser (for email and time wasting), and then several terminals that I use git in. With the occasional "gitk" window to show the git history view. Most of my time goes to reading (and answering) email, and merging trees and looking at the results.

我只开一个浏览器(收发邮件和打发时间），和在几个终端中跑git。偶尔会使用gitk窗口浏览git历史记录。大多数时间是读和回邮件，合并开发树以及查看结果。

> The other tool I tend to use is the "perf" tool to do performance profiling for the loads I care about (which is mainly kernel compiles and some git workloads).

其他使用的工具是perf，用于我关心的负载进行压力测试(主要有内核编译和一些git工作负载)。

> How did you get involved in Linux kernel development?

你是如何参入Linux内核开发的？

> Heh. Not enough common sense and knowledge to know that writing your own OS was a ridiculous amount of work.

呵。缺乏常识和写操作系统的知识，不可能胜任这份工作。

> What keeps you interested in it?

是什么让你对这份工作爱不释手？

> I still like the tinkering, and just the technical side of it. The fact that it's actually pretty social, and I get to call people names, is just a bonus.

我依旧喜欢解技术方面的问题。事实上这是相当现实的，？？？

> What's the most amused you've ever been by the collaborative development process (flame war, silly code submission, amazing accomplishment)?

在协作开发过程中你觉得好玩的是什么（嘴仗，愚蠢代码的提交，惊人的成就)？

> I think my favorite part is when somebody does something utterly crazy using Linux. Things that just make no sense at all, but are impressive from a technical angle (and even more impressive from a "they spent many months doing *that*?" angle).

我认为我喜欢的部分是某人使用Linux做一些完全疯狂的事情。从技术角度看，是一些毫无意义但又令人叹为观止的事情(甚至花费几个月干这事)。

> Like when Alan Cox was working on porting Linux to the 8086. Or the guy who built his own computer using an 8-bit microcontroller that he wired up to some RAM and an SD card, then wrote an ARM emulator for it, and booted Linux (really really slowly) on his board.

像Alan Cox移植Linux到8086上的工作。或者一些使用了拥有RAM和SD卡的8位微处理器来构建自己的计算机的家伙，在开发板上编写一个ARM仿真器，然后启动Linx(非常非常慢)。

> What's your advice for developers who want to get involved?

你对想进入该领域的开发者有什么建议？

> Start small. It doesn't even have to be Linux - there's a lot of open source projects that need help, and you want to learn how to get involved. And once you *do* realize that user-mode programmers are wusses, and you want to get involved with kernel programming, don't try to revolutionize some core kernel code - try to find some really small nagging concern, and fix that one thing. Maybe a driver for hardware that you have access to that doesn't work as well as it should, things like that.

慢慢来。不一定非要做Linux，有很多的开源项目需要帮助，你可以学习如何参与到其中。一旦你确实觉得用户模式的编程是在浪费时间，想要参与到内核开发中，不要尝试对内核代码进行大改，应该尝试找出一些小的问题，然后解决掉。一些像驱动程序没有如期的运行的问题。

> It takes a while to learn the ropes, and it really helps if people can see that you've done other things before you start sending more involved patches.

如果大家看到你在提交很多复杂的补丁前已经做了一些事情，花些时间学习协同开发是有帮助的。

> But the most important thing is "have good taste." It's hard to describe, but it's something I personally look for. People who do things the "RightWay(tm)" - and I'm not meaning that you should follow all the rules we have come up with over the years (although you should do that, too) - but I'm talking about that elusive quality of writing code that makes obvious sense and does the right thing without lots of special cases or complexity, but also without being unnecessarily abstract and general-purpose. "Do one thing, and do it well."

但最重要的是要有好的想法。这个是非常难描述的，却是我个人寻找的东西。做正确的事情的人，并不是说你应该遵守所有的之前建立的规则?????

> What do you listen to when you code?

你在编码的时候听什么？

> Oh, I want my office to be totally silent. I listen to music when I'm driving the kids around, etc., but when I'm working, I don't want to hear anything. Not music, not any noise from the fans in my computers. Just stillness.

我需要完全安静的办公室。当我驱车带孩子的时候才听音乐，工作时我不想听到任何声响。不能有音乐，电脑的风扇也不能有噪音。只要安静。

> What mailing list or IRC channel will people find you hanging out at? What conference(s)?

在哪个邮件列表或IRC频道可以找到你？会议呢？

> I don't do IRC or any other real-time interactive stuff - I do everything by email. I follow the general kernel and git mailing lists, but even those I have in "auto-archival" mode, so that I only see the threads if I explicitly look for them or if I'm cc'd or pointed at them.

我不使用IRC或其他的实时通讯工具，我只用邮件。我接收所有的内核和git邮件列表，但我有自动分析模式，这样我就可以查看我关注的或者我有被CC或要求查看的主题。

> As to conferences, it's usually just the Linux Kernel Summit. I try to get to LinuxConf Australia most years too - I like it as a conference and it's in Australia during their summer. But LCA is a "when it works out" kind of thing, so it's probably only every other year or so.

至于会议，只有Linux内核峰会。我有好几年尝试参加澳大利亚Linux会议。因为这是一个在澳大利亚的夏天举行的会议，所以我喜欢。但????

> There's been a few other conferences I go to, usually because they're in an interesting area and if I can get some scuba diving done during the same trip

还有其他一些会议，通常因为他们在一我感兴趣的领域以及在参与的过程????

> Thank you, Linus! Next week we talk to Thomas Gleixner. 

感谢你，Linus！下周我们对话Thomas Gleixner。
