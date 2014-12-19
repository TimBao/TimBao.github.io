---
layout: post
title: "More Effective C++ 读书笔记2"
date: 2013-01-05 09:42:00 
comments: true
categories: [C++技术]
tags: [C++技术]
description: "More Effective C++ 读书笔记2"
keywords: C++技术
---

    Item 2
   ：
  尽量使用C++风格的类型转换
   1. C语言的类型转换缺点
  1.1 过于粗鲁，允许在任何类型之间进行转换。
   1.2 在程序语句中难以识别。
    1.3 类型转换失败不可获知。
   2. C++类型转换符的介绍
  2.1.
   static_cast
  在功能上基本上与 C 风格的类型转换一样强大，含义也一样。但是，static_cast 不能从表达式中去除 const 属性。
  2.2.
   const_cast
  用于类型转换掉表达式的 const 或 volatileness 属性。
  2.3.
   dynamic_cast
  ，它被用于安全地沿着类的继承关系向下进行类型转换。在帮助你浏览继承层次上是有限制的。它不能被用于缺乏虚函数的类型上。
  2.4.
   reinterpret_cast
  ，其的转换 结果几乎都是执行期定义（implementation-defined）。因此， 使用reinterpret_casts 的代码很难移植。 reinterpret_casts 的最普通的用途就是在函数指针类型之间进行转换。
