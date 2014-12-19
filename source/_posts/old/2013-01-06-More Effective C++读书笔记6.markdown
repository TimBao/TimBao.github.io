---
layout: post
title: "More Effective C++读书笔记6"
date: 2013-01-06 09:22:00 
comments: true
categories: [C++技术]
tags: [C++技术]
description: "More Effective C++读书笔记6"
keywords: C++技术
---

   Item 6：自增(increment)、自减(decrement)操作符前缀形式与后缀形式的区别
 class UPInt
{
public:
    ...
    UPInt& operator++();         //前缀形式
    const UPInt operator++(int); //后缀形式
    UPInt& operator--();         //前缀形式
    const UPInt operator--(int); //后缀形式
};
UPInt& UPInt::operator++()
{
    *this += 1；
    return *this；
}
const UPInt UPInt::operator++(int)
{
    UPInt temp = *this;
    ++(*this);
    return temp;
}
...
  从代码中可以看到，
   1. 前缀是先增加再返回，而后缀是先返回再增加。
    2. 前缀和后缀在表现形式上就有所不同，是通过参数来区分的，这是C++语言规范规定的区别标准。
     3. 二者在返回值上也不相同，前缀形式返回类型的引用，而后缀则返回const 类型。为什么二者返回类型不相同，是为了防止出现 UPInt u； u++++；（在基础类型中不合法）。为了保持和基础类型一致而采取的考虑。
      4. 不管是前缀形式还是后缀形式，二者最终行为都是一致的（即增1），后缀实现采用前缀实现，这样就缩小改变，只需用改变前缀的实现就可以二者都改变。
       5. 从实现上可以看出，后缀形式比前缀形式多了一个临时对象temp，而临时对象会造成构造和析构的时间开销，空间方面也会有开销，这就导致前缀形式的效率会比后缀要高，所以能使用前缀形式就要使用前缀形式。
