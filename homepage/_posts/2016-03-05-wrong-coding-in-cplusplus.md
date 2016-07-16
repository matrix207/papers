---
layout: post
title: "Wrong coding in c++"
description: ""
category: language
tags: [c++]
---

### Use exception instead return boolean

Sample code as below:

    try {
        // Do something here, which may throw exception
    } catch (MyException& e) {
        if (e.getError() == MyExceptionA) {
            // Do another normal operation
        }
    }

### Throw exception in loop

### Reference
