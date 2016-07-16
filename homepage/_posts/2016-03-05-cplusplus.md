---
layout: post
title: "c++"
description: ""
category: language
tags: [c++]
---

### Books
* [C++FAQS](http://www.parashift.com/c++-faq-lite/)
* Junior
  - [(C++11)C++11 - 维基百科，自由的百科全书]()
  - [(C++11)深入理解C++11：C++ 11新特性解析与应用 迷你]()
  - [C++标准程序库--自修教程与参考手册]
  - [C++程序设计原理与实践]
  - [C++程序设计语言 特别版]
  - [C++编程惯用法——高级程序员常用方法和技巧]
  - [C++编程规范-101条规则准则与最佳实践]
  - [C++语言的设计和演化]
  - [C++释难解惑]
  - [C和C++安全编码（中文版）]
  - [Effective C++中文版（第三版）]
  - [More Effective C++ 中文版]
  - [[Effective STL中文版：50条有效使用STL的经验]]
  - [Exceptional C++ Style(Herb Sutter).中文版]
  - [Exceptional C++ 中文版]
  - [More Exceptional C++(中文版)]
  - [From C++ to Objective-C-english]
  - [From C++ to Objective-C-中文版]
  - [Linux C编程一站式学习6.14--宋劲衫]
  - [从缺陷中学习C、C++]
  - [你必须知道的222个C++语言问题.范立锋.扫描版]
  - [标准C++库函数参考]
  - [深入学习：GNU C++ for Linux 编程技术]
  - [编写高质量代码 改善C++程序的150个建 议.李健.扫描版]
* C/C++进阶
  - [Accelerated C++中文版 ]
  - [Advanced c++ Programming Styles and Idioms中文版]
  - [C++ API设计--英文版]
  - [C++ Templates中文版]
  - [C++ 工程实践经验谈--陈硕](http://www.cnblogs.com/Solstice/category/287661.html)
  - [C++ API设计]
  - [C++代码设计与重用]
  - [C++标准库——自学教程与参考手册(第2版)英文版]
  - [C++沉思录(Ruminations on C++)中文第2版]
  - [C++程序设计]
  - [C++设计新思维(Modern_C++_Design)]
  - [大规模C++程序设计]
  - [模板编程与OO编程范型--混搭]
  - [深度探索C++对象模型]
  - [高质量C++／C编程指南]
  - [Imperfect C++]
  - [C++ 并发编程指南]

### Projects
* 1.[leveldb](https://github.com/google/leveldb)
  - Google出品，遵循Google C++编码规范
  - 作者是Jeff Dean大神
  - 涉及查找、缓存、文件读写、多线程、跨平台等诸多常见问题
  - 最新版本1.15.0代码量也不到2万行
  - 基础的key-value数据库，广泛用于Google内部和许多开源项目
* [Google Test](http://code.google.com/p/googletest/)
* [Protocol Buffers](https://developers.google.com/protocol-buffers/)
* 2.[llvm](http://www.llvm.org/)
* 2.[muduo](https://github.com/google/leveldb)

### Lambda
* <http://zh.cppreference.com/w/cpp/language/lambda>

### resource manager
* Implement ScopeGuard
  - <https://en.wikibooks.org/wiki/More_C++_Idioms/Scope_Guard>
  - <http://mindhacks.cn/2012/08/27/modern-cpp-practices/>

* Sample code:

        // TODO: not work
        // g++ -o t -std=c++11 c++11.cc
        #include <stdio.h>
        #include <vector>
        #include <iostream>
        #include <algorithm>
        #include <functional>

        #define TT_HERE() {std::cout << __FUNCTION__ << ":" << __LINE__ << std::endl;}
        #define TT_PRINT_MSG(x) {std::cout << __LINE__ << ":" << x << std::endl;}

        class ScopeGuard
        {
        public:
            explicit ScopeGuard(std::function<void()> onExitScope)
                : onExitScope_(onExitScope), dismissed_(false)
            { }

            ~ScopeGuard()
            {
                TT_HERE();
                if(!dismissed_)
                {
                    onExitScope_();
                }
            }

            void Dismiss()
            {
                dismissed_ = true;
            }

        private:
            std::function<void()> onExitScope_;
            bool dismissed_;

        private: // noncopyable
            ScopeGuard(ScopeGuard const&);
            ScopeGuard& operator=(ScopeGuard const&);
        };

        #define SCOPEGUARD_LINENAME_CAT(name, line) name##line
        #define SCOPEGUARD_LINENAME(name, line) SCOPEGUARD_LINENAME_CAT(name, line)

        #define ON_SCOPE_EXIT(callback) ScopeGuard SCOPEGUARD_LINENAME(EXIT, __LINE__)(callback)

        class A
        {
        public:
            A () { TT_HERE(); }
            ~A () { TT_HERE(); }
        };

        void release()
        {
            std::cout << "do release" << std::endl;
        }

        class B
        {
        public:
            B () {
                TT_HERE();
            }
            ~B () { TT_HERE(); }
            void Fun1() {
                A a;
                std::cout << "do something in Fun1" << std::endl;
                std::cout << "throw exception" << std::endl;
                throw;
                release;
            }
        };

        class C
        {
        public:
            C () {
                TT_HERE();
            }
            ~C () { TT_HERE(); }
            void Fun1() {
                A a;
                ON_SCOPE_EXIT( [&] { release();} );
                std::cout << "do something in Fun1" << std::endl;
                std::cout << "throw exception" << std::endl;
                throw;
            }
        };

        void Test1()
        {
            std::vector<int> v;
            v.push_back(1);
            v.push_back(2);
            v.push_back(3);

            std::for_each(std::begin(v), std::end(v), [](int n) {std::cout << n << std::endl;});

            auto is_odd = [](int n) {return n%2==1;};
            auto pos = std::find_if(std::begin(v), std::end(v), is_odd);
            if(pos != std::end(v))
                std::cout << *pos << std::endl;
        }

        int main()
        {
            std::cout << "------------------------" << std::endl;
            B b;
            //b.Fun1();
            std::cout << "------------------------" << std::endl;
            C c;
            c.Fun1();
            std::cout << "------------------------" << std::endl;

            return 0;
        }

### Error handle
* Target:
  - easy to use
  - more error message
  - be strong
* Error information collection
  - information include: error, location and context
  - `ENSURE(0 <= index && index < v.size())(index)(v.size());`

        Failed: 0 <= index && index < v.size()  (error)
        File: xxx.cpp Line: 123                 (location)
        Context Variables:                      (context)
            index = 12345
            v.size() = 100

  - support multiple express `(ENSURE(expr)(var1)(var2)(var3))`
  - `static_assert(std::is_same<decltype(expr), bool>::value, "ENSURE(expr) can only be used on bool expression");`
* Implementation sample

        // Create by matrix207 2016-03-08
        // compile: g++ -o t -std=c++0x ensure.cc
        // expand macro with : g++ -E -P -std=c++0x ensure.cc
        #include <exception>
        #include <iostream>
        #include <sstream>

        class MyException
        {
        public:
            MyException(const char *exp, const char *file, int line)
                : SMART_ASSERT_A (*this),
                  SMART_ASSERT_B (*this)
            {
                std::ostringstream so;
                so << "Ensure failed : " << '\n'
                   << '\t' << "Expression : " << exp << '\n'
                   << '\t' << "Location : " << file << ':' << line << '\n';
                m_what += so.str();
            }
            template<typename T>
            MyException& printVariable(const char* name, T v)
            {
                std::ostringstream so;
                so << '\t' << name << " : " << v << '\n';
                m_what += so.str();
                return *this;
            }
            MyException& err(int error)
            {
                std::ostringstream so;
                so << '\t' << "Error : " << error << '\n';
                m_what += so.str();
                return *this;
            }
            MyException& msg(const char* msg)
            {
                std::ostringstream so;
                so << '\t' << "Message : " << msg << '\n';
                m_what += so.str();
                return *this;
            }
            const char *what() const throw ()
            {
                return m_what.c_str();
            }
            MyException& SMART_ASSERT_A;
            MyException& SMART_ASSERT_B;
        private:
            mutable std::string m_what;
        };

        #define SMART_ASSERT_A(x) SMART_ASSERT_AB(x, B)
        #define SMART_ASSERT_B(x) SMART_ASSERT_AB(x, A)
        #define SMART_ASSERT_AB(x,next) \
            SMART_ASSERT_A.printVariable(#x,(x)).SMART_ASSERT_ ## next

        #define MY_ENSURE(expr) \
            if (expr); else throw MyException(#expr,__FILE__,__LINE__).SMART_ASSERT_A

        int main()
        {
            try
            {
                int a = 2;
                MY_ENSURE(a > 3)(a).err(10001).msg("error occur!");
            }
            catch (const MyException& e)
            {
                std::cout << "catch exception:\n" << e.what() << std::endl;
            }
            return 0;
        }

* test

        [dennis@localhost code]$ g++ -g -o t -std=c++0x ensure.cc
        [dennis@localhost code]$ ./t
        catch exception:
        Ensure failed : 
            Expression : a > 3
            Location : ensure.cc:67
            a : 2
            Error : 10001
            Message : error occur!

### Reference
* <http://zh.cppreference.com/w/>
* <https://en.wikibooks.org/wiki/More_C++_Idioms>
* [Enhancing Assertions](http://www.drdobbs.com/cpp/enhancing-assertions/184403745)
* <http://www.cnblogs.com/cbscan/archive/2012/10/26/2740838.html>
