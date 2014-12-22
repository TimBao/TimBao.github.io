---
layout: post
title: "More Effective C++读书笔记19"
date: 2013-01-26 17:51:00
comments: true
categories: [C++技术]
tags: [C++技术]
description: "More Effective C++读书笔记19"
keywords: C++技术
---

##  Item 21：通过重载避免隐式类型转换

    如果是自定义类型进行隐式的类型转换，肯定会调用构造和析构函数，这样就一定会有一定的开销，那么如何避免这类隐式类型转换呢？21小节给出一个方式就是通过重载函数避免进行隐式类型转换。

```C++
class UInt
{
    public:
        const UInt operator+(const UInt& lrs, const UInt& hrs);
};
UInt a, b, c;
a = b + 10;
```

    在计算b+10的时候，编译器会将10隐式的转为UInt类型进行计算，这种情况下可以通过重载操作法来避免出现隐式转换的开销。当然是必要时候才会采取这种方法，如果不是性能瓶颈，那么有可能造成重载函数过多，而维护起来会很不方便。

    注意一种情况const UPInt operator+(int lhs, int rhs);   这种情况是错误的。在C++中有一条规则是每一个重载的operator必须带有一个用户定义类型（user-defined type）的参数。

##  Item 22：考虑用运算符的赋值形式（op=）取代其单独形式（op）

    在这一小节我个人来说学到了一点，那就是操作符重载中尽量能让实现能“集成”，不要所有实现都重新实现一遍，这样可以减少维护费用。
