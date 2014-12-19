---
layout: post
title: "More Effective C++ 读书笔记1"
date: 2013-01-05 09:42:00 
comments: true
categories: [C++技术]
tags: [C++技术]
description: "More Effective C++ 读书笔记1"
keywords: C++技术
---

  Item 1
 ：  指针和引用的区别
   1.指针可以为空值，引用不可以引用不可以为空值的好处是可以省略判断，提高代码效率。
  void Test（const  int& count）
{
    cout << count << endl;
}
void Test（const int* count）
{
    if(NULL != count)
    {
        cout << count << endl;
    }
}
  2 指针可以被改变，引用初始化后不可以再改变
   3 重载某些操作符时可能需要返回引用
    也就是说，当有可能会为空值的时候要使用指针，当有变量可能改变的时候要使用指针。
