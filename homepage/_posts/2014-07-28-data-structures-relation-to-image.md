---
layout: post
title: "data structures relation to image"
description: ""
category: tools
tags: [python,graphviz,iscsi,tools]
---

### What it is
**ds2img**.py (DataStructure To Image) 

A python script transfer c header file which contains lots of data structures 
to a dot script, and then use dot to convert to a image file

### How to run

	Usage: ds2imgpy -i INPUT_FILE -f png|svg -o OUTPUT_FILE [-d DOT_FILE]

	-i  input file which have structures"
	-f  image fomat, only support png and svg"
	-o  output file, image file"
	-d  dot script file, default is tmp.dot"

	e.g:

	python ds2img.py -i t.h -f png -o t.png 

### TODO
Waiting for your advices

### Warning
1. Not support structure contain other structure!
2. Not support multi-lines comment

### Source
You can get the latest version from: 
<https://github.com/matrix207/scripts/blob/master/ds2img/ds2img.py>

	#!/usr/bin/python
	#####################################################################
	# Generate dot script file from c file, which have lots of structures
	#
	# You can get the latest version from: 
	# https://github.com/matrix207/scripts/blob/master/ds2img/ds2img.py
	#
	# Warning: Not support structure contain other structure!
	# 
	# Depends:
	#     1. python
	#     2. graphviz
	#
	# History:
	#    v1.0  2014-07-28  Dennis  implement generate_relation function
	#                              add parse option funtion
	#    v0.1  2014-07-27  Dennis  Create
	#####################################################################

	import os
	import re
	import sys
	import getopt
	import datetime

	version = "v1.0 Create by Dennis 2014-07-28"
	debug = 0

	def is_comment(line):
		if '\\' in line:
			return true
		print true

	# process each line
	def process(line):
		print line,

	def get_struct_name(line):
		print line,

	def get_datetime():
		today = datetime.datetime.now()
		print today.strftime('%Y-%m-%d %H:%M:%S')

	def generate_dot_header(output_file):
		if debug : 
			print "generating dot header"
		f=open(output_file,'a+')
		print>>f, "/**********************************************"
		print>>f, "* Auto generate by ds2img.py"
		print>>f, "* Author:  matrix207"
		print>>f, "* Date  :  %s" % datetime.datetime.now().strftime('%Y-%m-%d %H:%M:%S')
		print>>f, "**********************************************/\n"
		print>>f, "digraph DS2IMG {"
		print>>f, "	node [shape=record fontsize=12 fontname=Courier style=filled];"
		print>>f, "	edge[color=blue]; rankdir=LR;"
		print>>f, ""
		f.close()

	def generate_dot_end(output_file):
		if debug : 
			print "generating dot end"
		f=open(output_file,'a+')
		print>>f, "}"
		f.close()

	def generate_struct_header(output_file, struct_name):
		if debug : 
			print "generating struct header"
		f=open(output_file,'a+')
		print>>f, "subgraph cluster_%s {" % struct_name
		print>>f, "    node [shape=record fontsize=12 fontname=Courier style=filled];"
		print>>f, "    color = lightgray; style=filled; label = \"struct %s \"; edge[color=\"#2e3436\"];" % struct_name
		print>>f, "    node_%s [shape=record label=\"<f0>*** struct %s ***\\" % (struct_name, struct_name)
		f.close()

	def generate_struct_member(output_file, index, member_name):
		if debug : 
			print "generating struct member"
		f=open(output_file,'a+')
		print>>f, "|<f%d>%s\\n\\" % (index, member_name)
		f.close()

	def generate_struct_end(output_file):
		if debug : 
			print "generating struct end"
		f=open(output_file,'a+')
		print>>f, "\"];"
		print>>f, "}"
		print>>f, ""
		f.close()

	def generate_relation(output_file, structs_name, structs):
		if debug : 
			print "generating relation"
		f=open(output_file,'a+')
		print>>f, "#relation "
		# structs_name contain all structure name 
		# structs      contain all structures, include structure name and it's member name
		for a in structs_name:           # structs_name[a] is structure name
			for b in structs:            # b is structure name 
				for c in structs[b]:     # c is member index, structs[b][c] is member name
					# only match structure name
					tmp = structs[b][c].split(' ')
					#if structs_name[a] in structs[b][c]:
					if tmp[1]==structs_name[a]:
						#print "%s contain %s\n" % (b,structs_name[a])
						#print "%s:<f%d> -> %s:f0\n" % (b,c,structs_name[a])
						print>>f, "node_%s:<f%d> -> node_%s:f0;" % (b,c,structs_name[a])
		print>>f, ""
		f.close()

	def clean_multi_space(line):
		line = re.sub(r' +', ' ', line)
		return line

	def clean_array_size(line):
		line = re.sub(r'\[.+\]', '[]', line)
		return line

	def clean_specify_dirty(line):
		line = re.sub(r'\[.+<<.+\]', '[]', line)
		return line

	# handle line which has comment information
	def handle_comment(line):
		pos = line.find("/*")
		if pos > 0:
			line = re.sub(r'/\*.+\*/', '', line)
			line = line.strip()
		return line

	def struct2dot(input_file, output_file):
		generate_dot_header(output_file)
		reader = open(input_file, 'r')
		i = 1
		structs = {}
		structs_name = {}
		while True: 
			line = reader.readline()
			if not line: 
				break
			# skip the comment line
			pos = line.find("/")
			if pos == 0:
				continue
			if pos > 0:
				handle_comment(line)
			m = re.match('^struct (\w+) {$',line)
			if m: # Find structure start
				structs_name[i] = m.group(1)
				st_name = m.group(1)
				i += 1
				generate_struct_header(output_file, m.group(1))
				structs[m.group(1)] = {}
				j = 1
				while True:
					line = reader.readline()
					if not line: 
						break
					line = line.strip();
					if len(line) == 0:
						continue
					# skip the comment line
					pos = line.find("/")
					if pos == 0:
						continue
					if pos > 0:
						line = handle_comment(line)
					line = clean_specify_dirty(line)
					line = clean_multi_space(line)
					m = re.match('^};$',line)
					if m: # Find structure end
						generate_struct_end(output_file)
						break
					structs[st_name][j] = line
					generate_struct_member(output_file, j, line)
					j += 1
		reader.close()
		generate_relation(output_file, structs_name, structs)
		generate_dot_end(output_file)

	def dot2png(input_file, output_file):
		cmdline = "dot -Tpng " + input_file + " -o " + output_file
		if debug:
			print cmdline
		os.system(cmdline)

	def clean_file(input_file):
		cmdline = "> " + input_file
		os.system(cmdline)

	# Test Function
	def test(input_file, output_file):
		#find_comment(sys.argv[1])
		generate_dot_header(output_file)
		generate_struct_header(output_file, "func_test")
		generate_struct_member(output_file, 1, "int test_a_13;")
		generate_struct_member(output_file, 2, "int test_a_13;")
		generate_struct_member(output_file, 3, "int test_a_13;")
		generate_struct_end(output_file)
		generate_dot_end(output_file)

	def find_comment(input_file):
		reader = open(input_file, 'r')
		while True:
			line = reader.readline()
			if len(line) == 0:
				break
			pos = line.find("/*")
			if pos>=0:
				print line,
		reader.close()

	def usage(bin_file):
		print "Usage: %s -i INPUT_FILE -f png|svg -o OUTPUT_FILE [-d DOT_FILE]" % bin_file
		print "      -i  input file which have structures"
		print "      -f  image fomat, only support png and svg"
		print "      -o  output file, image file"
		print "      -d  dot script file, default is tmp.dot"
		print "  e.g:\n\tpython %s -i t.h -f png -o t.png" % bin_file
		sys.exit(1)

	if __name__ == '__main__':
		paramlen = len(sys.argv)
		
		config = {  
			"input":"",  
			"format":"",  
			"output":"",  
			"dotfile":"tmp.dot",  
		}  	
		opts, args = getopt.getopt(sys.argv[1:], 'hi:f:o:d:',
			[  
			'input=',
			'format=',
			'output=',
			'dotfile=',
			'help'  
			]  
		)	

		for option, value in opts:  
			if  option in ["-h","--help"]:  
				usage(sys.argv[0])
			elif option in ['--input', '-i']:  
				config["input"] = value  
			elif option in ['--output', '-o']:  
				config["output"] = value  
			elif option in ['--format', '-f']:  
				config["format"] = value  
			elif option in ['--dotfile', '-d']:  
				config["dotfile"] = value  
			else:
				usage(sys.argv[0])

		if config["input"] == "" or config["output"]=="" or config["format"]=="" :
			usage(sys.argv[0])
		

		clean_file(config["dotfile"])

		struct2dot(config["input"], config["dotfile"])

		# generate graphic
		filename = os.path.basename(sys.argv[1]) 
		png_file = filename + ".png"
		dot2png(config["dotfile"], config["output"])

		print "Done"

### example
transfer iscsitarget-1.4.20/kernel/iscsi.h 

	[dennis@localhost kernel]$ pwd
	/home/dennis/work/git/iet/src/iscsitarget-1.4.20/kernel
	[dennis@localhost kernel]$ ds2img.py -i iscsi.h -f png -o iscsi.png
	Done
	[dennis@localhost kernel]$ ds2img.py -i iscsi.h -f svg -o iscsi.svg
	Done

![](/assets/image/posts/iscsi_kernel_structs.png)

### Reference
* [★★★使用graphviz画数据结构](http://emacser.com/emacs_graphviz_ds.htm)
* [graphviz for Data Structures](http://www.graphviz.org/content/datastruct)
* [使用graphviz dot来画图表](http://gashero.iteye.com/blog/1748795)
* [Python正则表达式指南](http://www.cnblogs.com/huxi/archive/2010/07/04/1771073.html)
* [★★python文件操作字符串操作总结](http://blog.csdn.net/wangyezi19930928/article/details/25652295)
* [用python如何匹配注释](http://zhidao.baidu.com/link?url=lwqlXGEAznkWAc26v929RcbA-TuG_McqeQ2BgRWWXaiNS2KQPfU4LR-QdmJkn3eWb5Bfn6qA_7wAboaFThjUkSznQi432soyDnXbd3vPxWO)
* [Python文件去除注释](http://blog.csdn.net/xmnathan/article/details/4192821)
* [★★★遍历python字典几种方法](http://5iqiong.blog.51cto.com/2999926/806230)
* [python循环遍历字典元素](http://www.cnblogs.com/skyhacker/archive/2012/01/27/2330177.html)
* [Python的字典操作](http://blog.csdn.net/nrs12345/article/details/4869272)
* [★★★python解析ini文件](http://blog.csdn.net/feixin620/article/details/4209783)
* [python_getopt解析命令行输入参数的使用](http://blog.csdn.net/xiaocaiju/article/details/7590106)

