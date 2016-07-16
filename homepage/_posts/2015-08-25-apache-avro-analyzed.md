---
layout: post
title: "apache avro analyzed"
description: ""
category: source
tags: [c,database]
---

#### analyzed apace avro c code

	////////////////////////////////////////////////////////////
	// Apache Avro c实现代码分析
	// 
	// By matrix207 2015-08-03 at home
	// 
	////////////////////////////////////////////////////////////

	/////////////////////// Part 1 /////////////////////////////
	文件                             功能
	--------------------------------------------------------------------------
	allocation.c allocation.h   一个提供申请、追加和释放内存功能的接口实现
								参考Lua的解析器实现
	array.c
	avroappend.c                例子?
	avrocat.c                   例子?
	avro_generic_internal.h
	avro.h                      include一些头文件
	avromod.c                   应该是测试压缩一个schema文件的例子?
	avropipe.c                  例子?
	avro_private.h              一些宏,感觉没有特别用处
	codec.h codec.c             编码器, 支持三种压缩算法:DEFLATE、LZMA和SNAPPY
	consume-binary.c
	consumer.h consumer.c       消费者???
	datafile.c
	datum.h datum.c             
	datum_equal.c
	datum_read.c
	datum_size.c
	datum_skip.c
	datum_validate.c
	datum_value.c
	datum_write.c
	dump.c
	dump.h
	encoding_binary.c
	encoding.h
	errors.h errors.c           错误信息，支持设置和提取功能，支持在已有的错误信息前插入新的错误信息.
	generic.h generic.c         内容很多,还要继续研究????
	io.h io.c                   io类型，包含文件io和内存io
	map.c                       map数据操作的实现
	memoize.c                   avro_memoize_t数据类型操作的实现
	resolved-reader.c
	resolved-writer.c
	resolver.h resolver.c
	schema.h schema.c
	schema_equal.c
	schema_specific.c
	st.h st.c                   通用hash表实现
	string.c                    使用wrapper buffer的方式创建一个类似string的数据avro_raw_string_t
								这里主要实现相关操作
	value.h value.c
	value-hash.c                value的hash操作，功能是??
	value-json.c                把二进制格式数据(datum)转化为支持UTF-8的JSON格式数据(avrocat中有使用)
	value-read.c                定义方法avro_value_read, 供datafile.c使用,avro_file_writer_open用到
	value-sizeof.c              定义方法avro_value_sizeof, 没有找到使用者???
	value-write.c               定义方法avro_value_write,供datum_write.c和datafile.c使用
	wrapped-buffer.c            buffer操作

	msinttypes.h                windows平台文件
	platform.h                  平台文件, 针对不同平台include不同的头文件
	basics.h                    简单和复杂数据类型的枚举型定义、schema和datum类型定义、对象类型定义
								各种宏来判断数据类型(简单和复杂类型)
	data.h                      各种数据类型的结构体定义(array、Maps、Wrapped buffer、strings、Memoization
	legacy.h                    为了兼容旧接口而保留的接口(新代码不会使用)
	msstdint.h                  windows平台文件
	refcount.h                  引用计数
	schema.h                    一些函数声明而已

	////////////////////////////////////////////////////////////
	重点，理解这些代码, from encoding_binary.c

	static int write_long(avro_writer_t writer, int64_t l)
	{
		char buf[MAX_VARINT_BUF_SIZE];
		uint8_t bytes_written = 0;
		uint64_t n = (l << 1) ^ (l >> 63);
		while (n & ~0x7F) {
			buf[bytes_written++] = (char)((((uint8_t) n) & 0x7F) | 0x80);
			n >>= 7;
		}
		buf[bytes_written++] = (char)n;
		AVRO_WRITE(writer, buf, bytes_written);
		return 0;
	}


