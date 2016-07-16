---
layout: post
title: "check pow overflow"
description: ""
category: program
tags: [c++]
---

### Source code

    // Create by matrix207 2016-03-09
    // g++ -o t check-pow-overflow.cc

    #include <iostream>
    #include <typeinfo>
    #include <cmath>
    #include <limits>

    /*
     * Check if pow overflow for type T (int,float,long,double)
     * a^b <= m  <=> log(a^b) <= log(m) <=> b*log(a) <= log(m) <=> b <= log(m)/log(a)
     * @param a
     *     base number
     * @param b
     *     exponent number
     * @return
     *     1 if overflow, otherwise 0
     */
    template<typename T1, typename T2>
    int check_pow(T1 a, T2 b)
    {
        int ret = 0;
        // i:int, f:float, d:double, l:long
        std::cout << "Type: " << typeid(a).name() << "," << typeid(b).name() << " ";
        double max_a = std::log(std::numeric_limits<T1>::max());
        double max_b = std::log(std::numeric_limits<T2>::max());
        double max_max = max_a < max_b ? max_b : max_a;
        if (b * std::log(a) < max_max) {
            double c = std::pow(a, b);
            std::cout << a << "^" << b << "=" << c << std::endl;
        } else {
            std::cout << a << "^" << b << " overflow" << std::endl;
            ret = 1;
        }
        return ret;
    }

    int main(int argc, char* argv[])
    {
        std::cout << "\n>>> Test int type <<<" << std::endl;
        check_pow(25, 1);
        check_pow(25, 10);
        check_pow(25, 100);
        check_pow(25, 1000);

        std::cout << "\n>>> Test long type <<<" << std::endl;
        check_pow(25L, 1L);
        check_pow(25L, 10L);
        check_pow(25L, 100L);
        check_pow(25L, 1000L);

        std::cout << "\n>>> Test float type <<<" << std::endl;
        check_pow(25.3f, 1.0f);
        check_pow(25.3f, 10.0f);
        check_pow(25.3f, 100.0f);
        check_pow(25.3f, 1000.0f);

        std::cout << "\n>>> Test double type <<<" << std::endl;
        check_pow(25.3, 1.0);
        check_pow(25.3, 10.0);
        check_pow(25.3, 100.0);
        check_pow(25.3, 1000.0);

        std::cout << "\n>>> Test mix type <<<" << std::endl;
        check_pow(25, 10.0); // int, double
        check_pow(25, 100.0); // int, double
        check_pow(25, 1000.0); // int, double
        check_pow(25.0f, 10.0); // float, double
        check_pow(25.0f, 100.0); // float, double
        check_pow(25.0f, 1000.0); // float, double

        std::cout << "\n>>> Test others <<<" << std::endl;
        check_pow(0, 100);
        check_pow(0, -2);
        check_pow(-1, -2);
        check_pow(-1, -20);
        check_pow(25, -20);

        return 0;
    }

### Test

    [dennis@localhost code]$ g++ -o t check-pow-overflow.cc
    [dennis@localhost code]$ ./t

    >>> Test int type <<<
    Type: i,i 25^1=25
    Type: i,i 25^10 overflow
    Type: i,i 25^100 overflow
    Type: i,i 25^1000 overflow

    >>> Test long type <<<
    Type: l,l 25^1=25
    Type: l,l 25^10=9.53674e+13
    Type: l,l 25^100 overflow
    Type: l,l 25^1000 overflow

    >>> Test float type <<<
    Type: f,f 25.3^1=25.3
    Type: f,f 25.3^10=1.0745e+14
    Type: f,f 25.3^100 overflow
    Type: f,f 25.3^1000 overflow

    >>> Test double type <<<
    Type: d,d 25.3^1=25.3
    Type: d,d 25.3^10=1.0745e+14
    Type: d,d 25.3^100=2.05141e+140
    Type: d,d 25.3^1000 overflow

    >>> Test mix type <<<
    Type: i,d 25^10=9.53674e+13
    Type: i,d 25^100=6.22302e+139
    Type: i,d 25^1000 overflow
    Type: f,d 25^10=9.53674e+13
    Type: f,d 25^100=6.22302e+139
    Type: f,d 25^1000 overflow

    >>> Test other <<<
    Type: i,i 0^100=0
    Type: i,i 0^-2 overflow
    Type: i,i -1^-2 overflow
    Type: i,i -1^-20 overflow
    Type: i,i 25^-20=1.09951e-28

### Reference
* [pow C++ Reference](http://www.cplusplus.com/reference/cmath/pow/)
* [log C++ Reference](http://www.cplusplus.com/reference/cmath/log/)
* [std::pow](http://en.cppreference.com/w/cpp/numeric/math/pow)
* [std::numeric_limits](http://en.cppreference.com/w/cpp/types/numeric_limits)
* <http://stackoverflow.com/questions/18609085/how-can-i-check-if-stdpow-will-overflow-double>
