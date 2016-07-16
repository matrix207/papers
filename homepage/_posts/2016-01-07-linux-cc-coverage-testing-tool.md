---
layout: post
title: "Linux c/c++ coverage testing tool"
description: ""
category: program 
tags: [c++, test]
---
#### gcov & lcov
* Reference <http://sdet.org/?p=212>
* step 1: add optional in Makefile, and build with `make coverage=yes`
    ifeq ($(coverage), yes)
    CXXFLAGS  += -fprofile-arcs -ftest-coverage
    LINKERCXX += -fprofile-arcs -ftest-coverage
    OPT_FLAGS  = -g3
    endif
* step 2: run test files
* step 3: generate text result by `gcov source_file.cc`, see result with `vim source_file.cc.gcov`
* step 4: generate html result by `lcov -c -d ./ -o app.info` and `genhtml app.info -o cc_result`

#### Reference
* [Linux下c/c++项目代码覆盖率的产生方法](http://sdet.org/?p=212)
