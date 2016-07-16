---
layout: post
title: "speed for getting info from pipe and proc system"
description: ""
category: program
tags: [performance]
---

### Test code
compile with `gcc -Wall -o test test.c`

The include file time_test.h, can be got frome 
<https://github.com/matrix207/C/blob/master/util/time_test.h>

	#include <stdio.h>  
	#include <stdlib.h>  
	#include <string.h>  
	#include <pthread.h>
	#include <sys/types.h>  
	#include <sys/stat.h>  
	#include <fcntl.h>  
	#include <unistd.h>  
	#include <signal.h>  
	#include <sys/socket.h>
	#include <sys/wait.h>
	#include <arpa/inet.h>
	#include <netinet/in.h>
	#include <net/if.h>
	#include <sys/ioctl.h>
	#include "time_test.h"

	int exec_cmd(const char *cmd, char *result, const int size)
	{
		FILE *fp = NULL;

		if (cmd == NULL) {
			printf("cmd can not be NULL\n");
			return 1;
		}

		if (NULL == (fp = popen(cmd, "r"))) {
			fprintf(stdout, "Fail to do %s\n", cmd);
			return 1;
		}

		memset(result, 0, size);
		printf("-------1-------\n");
		fgets(result, size, fp);
		printf("-------2-------\n");

		pclose(fp);
		return 0;
	}

	void get_from_proc(char *interface)
	{
		int sockfd;
		struct ifreq ifr;
		char mac_addr[30]={0};

		sockfd = socket(AF_INET, SOCK_STREAM, 0);
		if( sockfd == -1) {
			perror("create socket falise...mac\n");
			return; 
		}

		memset(&ifr,0,sizeof(ifr));
		strncpy(ifr.ifr_name, interface, sizeof(ifr.ifr_name)-1);

		if ((ioctl(sockfd, SIOCGIFHWADDR, &ifr)) < 0) {
			printf("mac ioctl error\n");
			return;
		}

		snprintf(mac_addr, sizeof(mac_addr), "%02x%02x%02x%02x%02x%02x",  
				(unsigned char)ifr.ifr_hwaddr.sa_data[0],
				(unsigned char)ifr.ifr_hwaddr.sa_data[1],
				(unsigned char)ifr.ifr_hwaddr.sa_data[2],
				(unsigned char)ifr.ifr_hwaddr.sa_data[3],
				(unsigned char)ifr.ifr_hwaddr.sa_data[4],
				(unsigned char)ifr.ifr_hwaddr.sa_data[5]);

		printf("local mac:%s \n",mac_addr);

		char *address;
		struct sockaddr_in *addr;
		if (ioctl(sockfd,SIOCGIFADDR,&ifr) == -1)
			perror("ioctl error"),exit(1);
		addr = (struct sockaddr_in *)&(ifr.ifr_addr);
		address = inet_ntoa(addr->sin_addr);
		printf("inet addr: %s \n",address);

		close(sockfd);
	}

	void get_from_pipe(char *interface)
	{
		char cmd[100] = {0};
		/* char *format="ifconfig %s |grep %s |awk '{print $5}'"; */
		char *format="ifconfig %s |grep ether |awk '{print $2}'";
		snprintf(cmd, sizeof(cmd), format, interface, interface); 
		char result[150] = {0};
		if (0 == exec_cmd(cmd, result, sizeof(result))) {
			printf("mac is %s\n", result);
		} else {
			printf("Fail\n");
		}
	}

	void time_test(int argc,char **argv)
	{
		/* XXX: change this for your computer */
		char *interface = "p4p1";
		if (argc == 2)
			interface = argv[1];
		
		TIME_START();
		printf("---------from proc system---------\n");
		get_from_proc(interface);
		TIME_END();

		TIME_START();
		printf("---------from pipe---------\n");
		get_from_pipe(interface);
		TIME_END();
	}

	int main(int argc,char **argv)
	{
		time_test(argc, argv);
		return 0;
	}

### Test detail

Using strace to log system calling time (output to file 1.log)

`strace -tT -o 1.log ./test`

The log info we care list as below:

	11:00:47 write(1, "---------from proc system-------"..., 35) = 35 <0.000030>
	11:00:47 socket(PF_INET, SOCK_STREAM, IPPROTO_IP) = 3 <0.000049>
	11:00:47 ioctl(3, SIOCGIFHWADDR, {ifr_name="p4p1", ifr_hwaddr=00:23:ae:96:d6:9a}) = 0 <0.000012>
	11:00:47 write(1, "local mac:0023ae96d69a \n", 24) = 24 <0.000018>
	11:00:47 ioctl(3, SIOCGIFADDR, {ifr_name="p4p1", ifr_addr={AF_INET, inet_addr("172.16.50.39")}}) = 0 <0.000013>
	11:00:47 write(1, "inet addr: 172.16.50.39 \n", 25) = 25 <0.000021>
	11:00:47 close(3)                       = 0 <0.000019>
	11:00:47 write(1, "Elapsed time 471 usec\n", 22) = 22 <0.000017>
	11:00:47 write(1, "---------from pipe---------\n", 28) = 28 <0.000018>
	11:00:47 brk(0)                         = 0x1af3000 <0.000009>
	11:00:47 brk(0x1b14000)                 = 0x1b14000 <0.000010>
	11:00:47 brk(0)                         = 0x1b14000 <0.000007>
	11:00:47 pipe2([3, 4], O_CLOEXEC)       = 0 <0.000014>
	11:00:47 clone(child_stack=0, flags=CLONE_CHILD_CLEARTID|CLONE_CHILD_SETTID|SIGCHLD, child_tidptr=0x7f6bff5eaa10) = 7658 <0.000077>
	11:00:47 close(4)                       = 0 <0.000008>
	11:00:47 fcntl(3, F_SETFD, 0)           = 0 <0.000007>
	11:00:47 write(1, "-------1-------\n", 16) = 16 <0.000020>
	11:00:47 fstat(3, {st_mode=S_IFIFO|0600, st_size=0, ...}) = 0 <0.001732>
	11:00:47 mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f6bff603000 <0.000016>
	11:00:47 read(3, "00:23:ae:96:d6:9a\n", 4096) = 18 <0.003692>
	11:00:47 write(1, "-------2-------\n", 16) = 16 <0.000170>
	11:00:47 close(3)                       = 0 <0.000009>
	11:00:47 wait4(7658, [{WIFEXITED(s) && WEXITSTATUS(s) == 0}], 0, NULL) = 7658 <0.000043>
	11:00:47 --- SIGCHLD {si_signo=SIGCHLD, si_code=CLD_EXITED, si_pid=7658, si_status=0, si_utime=0, si_stime=0} ---
	11:00:47 munmap(0x7f6bff603000, 4096)   = 0 <0.000018>
	11:00:47 write(1, "mac is 00:23:ae:96:d6:9a\n", 25) = 25 <0.000018>
	11:00:47 write(1, "\n", 1)              = 1 <0.000014>
	11:00:47 write(1, "Elapsed time 6639 usec\n", 23) = 23 <0.000022>

find the max 10 time by `awk '{print $NF}' 1.log |sort |tail`

	<0.000021>
	<0.000022>
	<0.000030>
	<0.000043>
	<0.000049>
	<0.000077>
	<0.000164>
	<0.000170>
	<0.001732>
	<0.003692>

then find the time killer `grep -E "001732|003692" 1.log`

	11:00:47 fstat(3, {st_mode=S_IFIFO|0600, st_size=0, ...}) = 0 <0.001732>
	11:00:47 read(3, "00:23:ae:96:d6:9a\n", 4096) = 18 <0.003692>

so, we know that the killer code is 

	fgets(result, size, fp);

If we using much code for our programming, and time is what we should care about,  
then avoid pipe code as possible as we can.

### TODO
1. Is each system call have fixed time?(ofcourse, there should be a litter 
   different for each testing)
2. Sort the time elapse for all the system call

### Reference

