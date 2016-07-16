---
title: understand gcc assembly output
date: 2016-04-24 19:02:22
description: "understand assembly helps to optimize performance"
category: language
tags: [gcc, assembly]
---

### 0x00 generate assembly code when compile
`gcc -S -o test.s test.c`

sample code

{% codeblock %}
	#include <stdio.h>

	int main(int argc, char* argv[])
	{
		int i = 0;
		for (i=0; i<10; i++) {
			printf("hello123");
		}
		return 0;
	}
{% endcodeblock %}

assembly code:

{% codeblock %}
		.file	"test.c"
		.section	.rodata
	.LC0:
		.string	"hello123"
		.text
		.globl	main
		.type	main, @function
	main:
	.LFB0:
		.cfi_startproc
		pushq	%rbp
		.cfi_def_cfa_offset 16
		.cfi_offset 6, -16
		movq	%rsp, %rbp
		.cfi_def_cfa_register 6
		subq	$32, %rsp
		movl	%edi, -20(%rbp)
		movq	%rsi, -32(%rbp)
		movl	$0, -4(%rbp)
		movl	$0, -4(%rbp)
		jmp	.L2
	.L3:
		movl	$.LC0, %edi
		movl	$0, %eax
		call	printf
		addl	$1, -4(%rbp)
	.L2:
		cmpl	$9, -4(%rbp)
		jle	.L3
		movl	$0, %eax
		leave
		.cfi_def_cfa 7, 8
		ret
		.cfi_endproc
	.LFE0:
		.size	main, .-main
		.ident	"GCC: (Ubuntu 4.8.4-2ubuntu1~14.04.1) 4.8.4"
		.section	.note.GNU-stack,"",@progbits
{% endcodeblock %}

### 0x01 basic acknowledge of assembly [AT&T]
* Register
* Move

### 0x02 Reference
* [understanding C by learning assembly](https://www.recurse.com/blog/7-understanding-c-by-learning-assembly)
* [X86汇编语言学习手记(1)](http://blog.csdn.net/yayong/article/details/170842)
* [X86 Assembly Guide](http://www.cs.virginia.edu/~evans/cs216/guides/x86.html)
* <https://en.wikibooks.org/wiki/X86_Assembly/GAS_Syntax>
