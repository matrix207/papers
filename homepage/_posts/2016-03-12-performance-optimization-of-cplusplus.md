---
layout: post
title: "performance optimization of C++"
description: ""
category: language
tags: [c++,performance]
---

### Tools for detect time killer
* [OProfile](http://oprofile.sourceforge.net/news/)
* [PAPI](http://icl.cs.utk.edu/papi/)

### Tips for Performance Improvement Code Optimization
* Optimize Your Code using Appropriate Algorithm
* Optimize Your Code for Memory
* Using operators
* If Condition Optimization
  - `if (a && b)`， place `b` first if `b` is more likely to be false
* Problems with Functions
* Optimizing Loops
  - It is faster to test if something is equal to zero than to compare two
    different numbers.
* Reference
  - <http://www.thegeekstuff.com/2015/01/c-cpp-code-optimization>
  - <http://stackoverflow.com/questions/2030189/general-c-performance-improvement-tips>

### memory allocate on stack or heap
Allocate memory on stack is more faster then heap cause of different architecture.
The heap is a far more complicated data structure than the stack.

**Ref:**

- <http://stackoverflow.com/questions/2264969/why-is-memory-allocation-on-heap-much-slower-than-on-stack>
- <http://stackoverflow.com/questions/4974617/what-is-more-efficient-stack-memory-or-heap>
- <http://stackoverflow.com/questions/161053/which-is-faster-stack-allocation-or-heap-allocation>

### vector vs array
* Have same assembly instruction
  - index operation is same
  - dereference operation is same
  - increment operation is same
  - <http://stackoverflow.com/questions/381621/using-arrays-or-stdvectors-in-c-whats-the-performance-gap>
* But there are some attributes make vector slower than C array
  - Vector possible to store data in non-continuous chunk of memory
  - Vector allocate memory on heap always
  - <http://assoc.tumblr.com/post/411601680/performance-of-stl-vector-vs-plain-c-arrays>
* Another performance test between vector , boost array and C array
  - <http://www.cnblogs.com/mudoot/articles/boost_array_0001.html>

### Use the right container
* more container compare and select
  - vector is good at read data
  - list is good at insert and remove data
  - deque has good balance for read, insert and remove data
  - <http://www.cppblog.com/sailing/articles/161659.html>
* reference
  - <http://stackoverflow.com/questions/3664272/is-stdvector-so-much-slower-than-plain-arrays>

### Some tips when using C++
```
class Foo
{
    const BigObject & bar();
};

BigObject obj = foo.bar();  // OOPS!  This creates a copy!
const BigOject &obj = foo.bar();  // does not create a copy
```

### some books
* Effective C++
* More Effective C++
* Effective STL
* C++ Coding Standards
* Efficient C++: Performance Programming Techniques

### Know some compile and linker options
* gcc

### Reference
* **<https://en.wikibooks.org/wiki/Optimizing_C++>**
* **<http://agner.org/optimize/optimizing_cpp.pdf>**
* <http://www.artima.com/cppsource/how_to_go_slow.html>
* <https://github.com/fffaraz/awesome-cpp>
* <https://notabug.org/koz.ross/awesome-c>
* <https://github.com/jobbole/awesome-c-cn>
