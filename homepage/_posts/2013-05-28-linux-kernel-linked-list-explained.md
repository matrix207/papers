---
layout: post
title: "Linux Kernel Linked List Explained"
description: ""
category: linux
tags: [kernel]
---

Using the list:

	#include <stdio.h>
	#include <stdlib.h>

	#include "list.h"

	struct kool_list{
		int to;
		struct list_head list;
		int from;
		};
																					
	int main(int argc, char **argv){
		struct kool_list *tmp;
		struct list_head *pos, *q;
		unsigned int i;
																					
		struct kool_list mylist;
		INIT_LIST_HEAD(&mylist.list);
																					
		for(i=5; i!=0; --i){
			tmp=(struct kool_list*)malloc(sizeof(struct kool_list));
			printf("enter to and from:");
			scanf("%d %d", &tmp->to, &tmp->from);
			list_add(&(tmp->list), &(mylist.list));
		}
		printf("\n");
																					
		printf("traversing the list using list_for_each_entry()\n");
		list_for_each_entry(tmp, &mylist.list, list)
			printf("to=%d from=%d\n", tmp->to, tmp->from);
		printf("\n");
																					
		printf("deleting the list using list_for_each_safe()\n");
		list_for_each_safe(pos, q, &mylist.list){
			tmp=list_entry(pos, struct kool_list, list);
			printf("freeing item to=%d from=%d\n", tmp->to, tmp->from);
			list_del(pos);
			free(tmp);
		}
																					
		return 0;
	}

Testing, as below:

	[dennis@localhost test]$ ./main
	enter to and from:19 9
	enter to and from:18 8
	enter to and from:17 7
	enter to and from:16 6
	enter to and from:15 5

	traversing the list using list_for_each_entry()
	to=15 from=5
	to=16 from=6
	to=17 from=7
	to=18 from=8
	to=19 from=9

	deleting the list using list_for_each_safe()
	freeing item to=15 from=5
	freeing item to=16 from=6
	freeing item to=17 from=7
	freeing item to=18 from=8
	freeing item to=19 from=9
	[dennis@localhost test]$

How it work? go to the reference website for detail.


Reference:

* [Linux Kernel Linked List Explained](http://isis.poly.edu/kulesh/stuff/src/klist/)
