---
layout: post
title: "Linux protocol stack"
description: ""
category: network
tags: [kernel,network]
---

From <http://www.cnblogs.com/image-eye/category/347518.html>

### 网络协议栈0：从一个例子开始

最近因工作需要写一个网卡驱动，晕倒，没有任何网络知识，就写网络驱动！

可是，为了五斗米糊口，不得不从啊

于是，打算从网络协议栈开始，把网络搞一搞。

我们常常知道socket的用法（其实我还没有真正的写过socket代码，常常都是指那些socket高手了^-^），因此，打算从一个常用的实例开始，把网络协议栈整理一下，即把自己的学习经过进行记录，看看菜鸟的轨迹，是如何拐弯，颠簸。

通常的socket编程分两部分吧（错了别怪我，我不是高手），一是client部分，二是server部分，而更通常的情况是我们都以写client的任务为多，因此，从简单下手，当然选择client端开始了。

下面的代码，就是随便一个网站都能搜索到的client端的代码，我就毫不客气的复制了，谁叫我是菜鸟：

	#include <stdio.h>
	#include <sys/socket.h>
	#include <unistd.h>
	#include <sys/types.h>
	#include <netinet/in.h>
	#include <stdlib.h>
	#include <stdio.h>
	#include <stdlib.h>
	#include <errno.h>
	#include <string.h>
	#include <sys/types.h>
	#include <netinet/in.h>
	#include <arpa/inet.h>
	#include <sys/socket.h>
	#include <sys/wait.h>
	#include <unistd.h>
	#include <netdb.h>
	#include <time.h>


	#define SERVER_PORT 20000 // define the defualt connect port id
	#define CLIENT_PORT ((20001+rand())%65536) // define the defualt client port as a random port

	#define BUFFER_SIZE 255
	#define REUQEST_MESSAGE "welcome to connect the server./n"

	void usage(char *name)
	{
		printf("usage: %s destinationIpAddr\n",name);
	}

	int main(int argc, char **argv)
	{    
		int clifd,length = 0;
		struct sockaddr_in servaddr,cliaddr;
		socklen_t socklen = sizeof(servaddr);
		char *serv_ip="192.168.1.235";
		char buf[BUFFER_SIZE];

		/*       if (argc < 2)
				 {
				 usage(argv[0]);
				 exit(1);
				 }
		 */      
		if ((clifd = socket(AF_INET,SOCK_STREAM,0)) < 0)
		{
			printf("create socket error!/n");
			exit(1);
		}
		srand(time(NULL));//initialize random generator
		bzero(&cliaddr,sizeof(cliaddr));
		cliaddr.sin_family = AF_INET;
		cliaddr.sin_port = htons(CLIENT_PORT);
		cliaddr.sin_addr.s_addr = inet_addr("192.168.0.234");//htons(INADDR_ANY);

		bzero(&servaddr,sizeof(servaddr));
		servaddr.sin_family = AF_INET;
		inet_aton(serv_ip/*argv[1]*/,&servaddr.sin_addr);
		servaddr.sin_port = htons(SERVER_PORT);
		//servaddr.sin_addr.s_addr = htons(INADDR_ANY);

		if (bind(clifd,(struct sockaddr*)&cliaddr,sizeof(cliaddr))<0)
		{
			printf("bind to port %d failure!/n",CLIENT_PORT);
			exit(1);
		}

		if (connect(clifd,(struct sockaddr*)&servaddr, socklen) < 0)
		{
			printf("can't connect to %s!/n",/*argv[1]*/serv_ip);
			exit(1);
		}

		length = recv(clifd,buf,BUFFER_SIZE,0);
		if (length < 0)
		{
			printf("error comes when recieve data from server %s!",/*argv[1]*/serv_ip);
			exit(1);
		}
		printf("from server %s :/n/t%s ",/*argv[1]*/serv_ip,buf);
		int rc;
		rc=send(clifd,"some messages",strlen("some messages"),0);
		if(rc < 0)
		{
			printf("error comes when recieve data from server %s!",/*argv[1]*/serv_ip);
			exit(1);
		}
		close(clifd);
		return 0;
	}

 

标准的菜鸟格式，当然，我也是经过了编译，确认了代码的可执行(fedora6下)，并且跟服务器交互正常。

一样的三步曲，不一样的曲折。

socket编程，常规的就是

1.socket()

2.bind()

3.connect()

成功之后，就是随意的发送，接收数据了。

只是，上面这三步，干了什么活，就把两个互不相干的IP链接起来，这么牛？

### 网络协议栈13：Connect函数分解之链路层

Skb_buff数据包从IP层下传到链路层后，链路层开始对数据包进行处理

首先，判断skb_buff数据包是不是不在skb_buff链表中，如果还在（即skb_buff->next!=NULL），则说明上面的处理有问题，代码要避开（人为的避开，代码还是有问题），即不能发送这个数据包，处理方式是，指定发送数据包的设备被指定为NULL，即数据包没有设备真实发送。

第二步，判断是否已知下一跳的MAC地址，即skb->arp=1，如果不是，则需要调用arp_find来查找IP地址对应的MAC地址，如果找不到，则直接返回，不再进行发送。

第三步，前面两步都正常了，则说明数据包正常，此时判断数据是否是刚刚下传的本地数据包，是则把数据包按照优先级的级别再次进行排队，这里分三种优先级，每个优先级都是一个队列，数据包按照相应的优先级被插入对应优先级的队列的尾部。

第四步，从最高优先级队列头部获得一个数据包，这个数据包有可能不是刚刚下传的数据包，如果下传的数据包大于发送的数据包，或者有重传的数据包，则此时获得的数据包就是先前的数据包。

第五步，把刚获得数据包进行发送（调用网卡驱动中的发送函数进行发送，到这里才是网卡驱动工作的开始），如果发送成功，就直接返回了，如果发送不成功，则把这个数据包插入到相应优先级的队列的头部，后面发送时它是相应对了中最先得到发送的数据包。

 由于网卡各式各样，没有办法进行抽象，因此，网卡的驱动被开发出来，只需要调用相应的上层函数，再把网卡的功能集合到网络协议栈中，行程完整的协议栈。

其中，第二步中，如果目的端IP对应的MAC地址还没有（或者下一跳），会使用arp_find来查找对应IP地址对应的MAC地址，其大致过程是：

ARP会在本地有一个ARP缓存，所谓的ARP缓存，在实现上采用数组加链表（或者称为队列）的方式，每个 ARP缓存表项由一个 arp_table 结构表示，所以数组中每个元素就指向一个 arp_table 结构类型的链表。对于具体数组元素的寻址由被解析的 IP 地址通过 Hash 算法而得。所以查询 ARP 缓存的过程就可表述为：首先根据被解析 IP地址索引数组中对应元素指向的 arp_table 结构链表，然后遍历该链表，对 IP地址进行匹配，如果查找一个 IP地址精确匹配的表项，则返回该表项，如果没有寻找到，则表示当前 ARP 缓存中没有对应表项，系统此时将创建一个新的 ARP表项，并发出 ARP请求报文，而当前发送的数据包就被缓存到该表项设置的数据包缓存队列中，当接收到 ARP 应答报文时，一方面完成原先表项的创建（硬件地址字段的初始化） ，另一方面将该表项中之前缓存的所有待发送数据包发送出去。结构图如下：

Arp_table结构体如下：

	struct arp_table
	{
		struct arp_table     *next;            /*next 字段用于 arp_table结构之间的相互串接，构成一个链表。*/
		unsigned long        last_used;        /*last_used 字段表示该表项上次被使用的时间*/
		unsigned int         flags;            /*flags 字段用于维护 ARP 表项的一些标志位，如表项当前所处状态，可以为以后的 ARP 缓存表项的功能扩展做好准备。 */
		unsigned long        ip;               /*ip 字段表示表项所表示映射关系中的 IP地址*/
		unsigned long        mask;             /*mask 字段为对应 IP地址的网络掩码。*/
		unsigned char        ha[MAX_ADDR_LEN]; /*ha 字段表示映射关系中的硬件地址*/
		unsigned char        hlen;             /*hlen 为硬件地址长度*/
		unsigned short       htype;            /*htype 为硬件地址类型*/
		struct device        *dev;             /*dev 字段表示该 ARP 表项绑定的网络设备，对于只有一个网络接口的主机，所有数据包的发送当然只有一个发送通道，但对于具有两个或者更多网络接口的主机，数据包就有多种可能的选择，如果每次发送时都进行发送接口的查询会不必要的增加系统开销，由于数据帧创建中链路层首部的创建都需要进行硬件地址解析，即查询 ARP缓存，所以在 ARP表项中维护一个对应发送接口的指针， 就可以在进行数据帧创建时一并解决通过哪个网络接口发送数据包的问题。另外对于多个网络接口的主机，每个网口都接在不同的网络上，而远端主机只可能属于一个网络，所以我们在进行 ARP 表项创建时可以知道这个主机属于哪个网络，从而初始化接入该网络的网口设备指针。由此可见，ARP 缓存表项中 dev 字段值将来自于路由表项。而路由表项要么是手工配置，要么根据运行路由协议创建，而根据接收路由数据包的网络接口我们可以进行路由表项中网口设备字段的初始化。*/
		struct timer_list    timer;            /*timer 字段用于定时重发 ARP 请求数据包，以防止 ARP 请求未得到响应。*/
		int                  retries;          /*重发次数，当前设置的最大次数为 3，由 ARP_MAX_TRIES 变量表示*/
		struct sk_buff_head  skb;              /*最后一个字段 skb 表示暂时由于 IP 地址到硬件地址的解析未完成而被缓存的数据包队列该字段的具体使用在下文中介绍到相关函数时进行说明。*/   
	};

 

而arp缓存数组定义如下：

	#define ARP_TABLE_SIZE  16
	#define FULL_ARP_TABLE_SIZE (ARP_TABLE_SIZE+1)
	struct arp_table *arp_tables[FULL_ARP_TABLE_SIZE] =
	{
		NULL,
	};
 

系统就是通过上述的结构体和数组来管理ARP缓存的

### 网络协议栈14：Connect函数分解之网卡发送/接收数据流程

链路层经过对数据包的优先级进行排队后，把本次需要发送的数据包从优先级最高的队列的头部抽取出来，并下传给网卡驱动程序的数据发送函数，一般命名为xxx_hard_xmit()，由于硬件各个不同，写法也会各个不同，无法抽象出一个标准的写法，但其流程、目的是不变的，是可以抽像出来的，大致的流程如下：
1．通过设备结构体net_device中的tbusy(transmit busy)来判断网卡现在是否忙，如果等于1，表示网卡在忙，即目前仍然有数据在传输数据，此时是不能传输数据的，而是判断是否超时，如果未超时，则表示网卡正在忙，正在发送数据包，此时直接返回；如果是超时了，则重新初始化相关寄存器，之后设置 tbusy=0，然后继续下面的步骤。
2．设置tbusy = 1,准备传输数据，因为经过这样的设置，下面如果有数据需要发送时，遇到tbusy = 1，就会像重复第一步的判断。
3．硬件开始传输数据包，此时，主要是通过对网卡的寄存器的操作来完成的，一般的，我们会提供需要传送的数据的地址，以及数据的长度，然后经过芯片的寄存器把这些数据发送出去。
4．发送数据是一个for循环进行，一直发送，直到数据发送完才结束。结束发送数据后，就要把刚刚传过来的数据包释放掉，把相关的内存回收，以备后面继续使用。
5．最后，修改设备的一些统计信息，完成本次传输。
 
到此，connect函数从数据包的层层打包，传输，到最后从网卡上发送出去的流程，就完成了本地的所有传输，此后的数据，经过以太网的转发，送往远端的目的IP地址，此时，connect函数就开始睡眠，等待确认数据从远方传递回来。

Connect函数在等待确认数据的到达，即需要解决的问题是，数据如何从网卡被上传到链路层。这个过程也因为网卡的硬件各个不同，而无法有标准写法，只有一个标准的流程。

1．当网卡接收到了一个完整的数据包后（硬件完成接收，而且是一个完整的数据包，即意味则网卡本身是有buffer的，是可以临时存储数据的），会发生一个中断信号给系统，这个中断信号是系统在启动时，由BIOS记录了跟一个中断处理程序关联到一起的，自然的，下面就是调用这个中断处理程序，来处理中断了。

2．通过设备结构体net_device中的interrupt成员来判断网卡现在是否忙,如果interrupt = 1，表示有其他进程在运行中断处理程序，则退出，否则，继续下面的步骤。

3．设置interrupt = 1，表示本进程在使用中断处理程序，其他的进程暂时不能使用。

4．读取中断状态寄存器，根据状态寄存器判断中断发生的原因。有两种原因，一种是有新的数据包到达，另一种是上次传输的数据已经传输完成。

5．如果有新的数据包到达，则调用接收数据包的子函数来接收数据。

6．如果是上次数据传输完成引起的，则通知协议的上层，修改接口的统计数据，关闭tbusy标志位，为下次传输做准备。

7．关闭标志位interrupt，表示本中断使用完成，现在其他进程可以使用中断处理函数了。

其中的第五步的调用子函数来接收数据的过程大致如下：

1．申请skb缓冲区给准备接收的数据包，用于存储数据包

2．从硬件中读取数据到skb缓冲区

3．调用netif_rx()函数，把数据送往链路层。

4．修改接口的统计数据。


### 网络协议栈15：网卡接收/发送数据基础知识

网卡本身是有内存的，每个网卡一般都有4K以上的内存，用来发送，接收数据。

数据在从主内存搬到网卡之后，不是立即就能被发送出去的，而是要先在网卡自身的内存中排队，再按照先后顺序发送；同样的，数据从以太网传递到网卡时，网卡也是先把数据存储到自身的内存中，等到收到一帧数据了，再经过中断的方式，告诉主CPU（不是网卡本身的微处理器）把网卡内存的数据读走，而读走后的内存，又被清空，再次被使用，用来接收新的数据，如此循环往复。
而网卡本身的内存，又多是按照256字节为1页的方式，把所有内存分页，之后把这些页组成队列，大致的结构如图：

一般会划分一小部分页面作为发送数据用的，大部分用于接收网络数据，大致如图：

蓝色部分为发送数据用的页面总和，总共只有6个页面用于发送数据（40h~45h）；剩余的46h~80h都是接收数据用的，而在接收数据内存中，只有红色部分是有数据的，当接收新的数据时，是向红色部分前面的绿色中的256字节写入数据，同时“把当前指针”移动到+256字节的后面（网卡自动完成），而现在要读的数据，是在“边界指针”那里开始的256字节（紫色部分），下一个要读的数据，是在“下一包指针”的位置开始的256字节，当256字节被读出来了，就变成了重新可以使用的内存，即绿色所表示，而接收数据，就是把可用的内存拿来用，即变成了红色，当数据写到了0x80h后，又从0x46h开始写数据，这样循环，如果数据满了，则网卡就不能再接收数据，必须等待数据被读出去了，才能再继续接收。


下面是一些网卡常用的寄存器：

CR(command register)---命令寄存器

TSR(transmit state register)---发送状态寄存器

ISR(interrupt state register)----中断状态寄存器

RSR(receive state register)---接收状态寄存器

RCR(receive configure register)---接收配置寄存器

TCR(transmit configure register)---发送配置寄存器

DCR(data configure register)---数据配置寄存器

IMR(interrupt mask register)---中断屏蔽寄存器

NCR(non-coding region)---包发送期间碰撞次数

FIFO(first in first out)

CNTR0(counter register)--- 帧同步错总计数器

CNTR1---CRC错总计数器

CNTR2---丢包总计数器

PAR0~5(physical address register)---本地MAC地址

MAR0~7(multiple address register)---多播地址匹配

PSTOP(page stop register)---结束页面寄存器

PSTART(page start register)---开始页面寄存器

BNRY(boundary register)----边界页寄存器

CURR(current page register)---当前页面寄存器

CLDA0,1(Current Local DMA Address)---当前本地DMA寄存器

TPSR(Transmit page start register)---传送页面开始寄存器

TBCR0,1(transmit byte counter register)---传送字节计数寄存器

CRDA0,1(current remote DMA address)---当前远程DMA寄存器

RSAR0,1(remote start address register)---远程DMA起始地址寄存器

RBCR0,1(remote byte counter register)---远程字节计数寄存器

BPAGE(BROM page register)---BROM页面寄存器

### 网络协议栈16：connect函数分解之链路层接收数据

Connect函数在发送了数据之后，就进入了休眠等待的状态，等待远方发过来的数据来确认链接是否成功。那么数据是如何从网卡那里传递到链路层？

数据在网卡的内存中，形成了以256字节为1页的环形缓冲链，每当有数据到达，网卡就会发生一个中断信号，告诉CPU已经有数据到达，此时，CPU通过DMA的方式，去读取网卡内存中的数据，并把数据存放在一个专门开辟来接收这个数据包的skb_buff中，之后，就把接收数据的skb_buff进行排列，并且累加所有skb_buff的个数，如果超过一定的数量在排队等待被取走，则后面不能再继续从网卡中读取数据包，以防止系统内存被过度的消耗掉。因此，在数据链路层，skb_buff是被排成一个队列，达到FIFO的目的。

一般接收数据都是在中断中完成，而中断时需要快速的进行处理，以免消耗系统过多的资源，因此，把数据进行队列排队后，就要离开中断处理函数了，此时，需要启动中断程序的后半部来进行剩余的数据的处理。 net_bh()函数是接收中断处理函数的后半部处理函数，其主要的工作是

1．把还在链路层中排队，还没有发送出去的数据包发送送出去（可见发送数据包是很重要的）。

2．把在接收队列中排队的数据包的上层协议类型都找出来，准备把数据发送往对应的协议层（遍历协议类型链表，即struct packet_type所组成的链表，来找到需要发送的数据包所对应的上层协议）。

3．调用相应的上层协议的接收函数来接收数据包。

上层协议的接收函数，是在操作系统初始化的时候，就已经初始化好了，其实就是注册

	struct packet_type {
	  unsigned short type;       /* This is really htons(ether_type). */
	  struct device *dev;
	  int (*func) (struct sk_buff *, struct device *, struct packet_type *);
	  void *data;
	  struct packet_type  *next;
	};

 

这样的结构体（比如static struct packet_type ip_packet_type，static struct packet_type arp_packet_type），其中的func指针所指向的，就是type所确定的协议类型的接收函数，type可取类型如下

	#define ETH_P_LOOP  0x0060           /* Ethernet Loopback packet     */
	#define ETH_P_ECHO  0x0200           /* Ethernet Echo packet         */
	#define ETH_P_PUP   0x0400           /* Xerox PUP packet             */
	#define ETH_P_IP    0x0800           /* Internet Protocol packet     */
	#define ETH_P_ARP   0x0806           /* Address Resolution packet    */
	#define ETH_P_RARP  0x8035           /* Reverse Addr Res packet      */
	#define ETH_P_X25   0x0805           /* CCITT X.25                   */
	#define ETH_P_ATALK 0x809B           /* Appletalk DDP                */
	#define ETH_P_IPX   0x8137           /* IPX over DIX                 */
	#define ETH_P_802_3 0x0001           /* Dummy type for 802.3 frames  */
	#define ETH_P_AX25  0x0002           /* Dummy protocol id for AX.25  */
	#define ETH_P_ALL   0x0003           /* Every packet (be careful!!!) */
	#define ETH_P_802_2 0x0004           /* 802.2 frames                 */
	#define ETH_P_SNAP  0x0005           /* Internal only                */

### 网络协议栈17：connect函数分解之网络层接收数据处理

链路层经过对上层协议的检查，把数据包上传到了对应的上层协议层，在这里，就是IP层协议。

数据到达IP层后，IP层需要进行相应的检查，判断后，才决定是否需要把数据上传到上层。下面就是IP层所需要做的事情.

1．首先检测数据包的IP首部是否正确，即对数据包的IP部分的IP首部长度，版本，数据包的大小进行检查，如果符合要求，则继续进行下面的步骤，不符合要求，则释放数据包，直接退出

2．检测IP首部是否包含了选项部分，方法就是查看IP首部的总长度字段是否大于IP首部的长度，如果是，则需要把选项部分解析，这个选项部分会在上传到上层协议时就告知是否有选项的。

3．检测数据包是否是一个分片数据，如果是一个分片数据，则需要分配一个空间，把这个数据包预存起来，同时等待所有数据包的到达后，把所有分片数据重组，才返回这个完整的数据包。如果不是，则直接返回，相当于等待所有分片数据并完成重组。

4．检测是否是多播的数据包，如果是多播的数据包，则再检测这个数据包是否在多播组中，如果是，就接收这个数据包，如果不是，则放弃接收。如何检测本地IP是否是多播组中的一员，主要是遍历当前设备的多播组链表，如当前设备所维护的多播组链表中是有本次传输的目的IP，则表示本IP是多播组的一员，可以接收数据。

5．检测本次数据包对应的上层协议，主要是遍历struct inet_protocol这个链表而得知，因为struct inet_protocol这个链表在系统初始化时，把igmp_protocol / icmp_protocol / udp_protocol / tcp_protocol 等等这些上层协议都链接好，现在只需要遍历，即可找到对以的上层协议，即可调用对应上层协议的接收函数来接收数据，对以connect函数，IP上面的协议就是TCP协议，其所对以的struct inet_protocol结构体：

	static struct inet_protocol tcp_protocol = {
		tcp_rcv,     /* TCP handler        */
		NULL,        /* No fragment handler (and won't be for a long time) */
		tcp_err,     /* TCP error control  */
		NULL,        /* next               */
		IPPROTO_TCP, /* protocol ID        */
		0,           /* copy               */
		NULL,        /* data               */
		"TCP"        /* name               */
	};

而接收函数就是tcp_rcv(),系统就是调用这个函数来接收IP上传的数据
