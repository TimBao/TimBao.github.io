---
layout: post
title: "More Effective C++读书笔记7"
date: 2013-01-06 11:39:00
comments: true
categories: [C++技术]
tags: [C++技术]
description: "More Effective C++读书笔记7"
keywords: C++技术
---

## Item 7：不要重载“&&”,“||”, 或“,”

C/C++中“&&”和“||”操作符是采用短路计算法的，而且是有顺序的。即从左往右计算，如果一旦确认表达式的值，那么后面的表达式或计算不再执行。而重载运算符本质上是函数重载，即运算符操作为函数操作。operator &&()有两种情况，一是全局重载，二是作为类成员函数；如果是全局重载，那么调用本质为 "operator &&（expression1， expression2）",而C++并没有规定参数的计算顺序，所以没有办法保证重载的&&保持原意不变。成员函数形式为expression1.operator&&(expression2)，跟全局问题一样，所以“&&”和“||”操作符适合重载。“，”运算符也是相同的原因，这里不再赘述。

操作符重载的目的是为了程序更容易阅读，理解和书写。
