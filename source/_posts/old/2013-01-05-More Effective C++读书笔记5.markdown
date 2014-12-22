---
layout: post
title: "More Effective C++读书笔记5"
date: 2013-01-05 12:18:00
comments: true
categories: [C++技术]
tags: [C++技术]
description: "More Effective C++读书笔记5"
keywords: C++技术
---

## Item 5：谨慎定义类型转换函数

隐式类型转换时在编译期间由编译器来执行的。除了C/C++默认的基本类型的隐式类型转换外，用户自定义类型的隐式转换方式有两种：

1.定义隐式类型转换运算符

2.单参数构造函数或者多参数构造函数，但第一个参数以后的参数都有默认值。

隐式类型转换运算符例子：
```
class Rational
{
public:
    Rational（int numerator = 0, int denominator = 1）；
    operator double() const;   //不需要返回类型，函数名称就是返回类型
};
Rational r（1， 2）；
double d = 0.5 * r; //r 转换为double类型
```
标题是要谨慎定义类型转换函数，为什么要谨慎呢，就是说这些定义的类型转换函数有可能会产生不是你想要的结果，比如你不想进行转换的时候，却给你转换了。如下情况：

cout << r；  //Rational并没有定义operator << 运算符，但是，编译器会找到operator double()转换函数，将r转换为double输出。但这并不是你想要的结果。

解决方法是用不使用语法关键字的等同的函数来替代转换运算符。如定义一个toDouble()函数来代替operator double()转换函数。STL中的string就提供了一个c_str()函数来转换到char*。

通过单参数构造函数进行隐式类型转换更难消除。而且在很多情况下这些函数所导致的问题要甚于隐式类型转换运算符。

我们来看一个例子：
```
temlplate<typename T>;
class Array
{
public:
    Array(int lowBound, int hightBound);
    Array(int size);
    T& operator[](int index);
};
Array<int>; a(10);
Array<int>; b(10);
for (int i = 0; i < 10; ++i)
{
    if (a == b[i]) // 哎呦! "a" 应该是 "a[i]"
    {
        do something for when
        a[i] and b[i] are equal;
    }
    else
    {
        do something for when they're not;
    }
}
```

我们的编译器注意到它能通过调用 Array<int>;构造函数能转换 int 类型到 Array<int>;类型，这个构造函数只有一个 int 类型的参数。然后编译器如此去编译，生成的代码就象这样：
```
if (a == static_cast< Array<int>; >;(b[i]))
```
解决方案是利用一个最新编译器的特性，explicit 关键字。
