---
layout: post
title: "More Effective C++读书笔记16"
date: 2013-01-16 10:13:00 
comments: true
categories: [C++技术]
tags: [C++技术]
description: "More Effective C++读书笔记16"
keywords: C++技术
---


 
  
   Item 17：考虑使用lazy evaluation（懒惰计算法）
  
 
 
  
  
 
 
  首先说说使用lazy evaluation的优点，主要是能提高程序效率。延时计算，有可能避免不必要的计算，或者是只需用计算部分结果，而不需要全部结果，这些都可以称为lazy evaluation。C++本身是early evaluation，它需要程序员自己实现lazy evaluation，这样大大增加了程序员的灵活性，可以根据实际情况使用lazy evaluation。
 
 
  
  
  书中提到可以在具体4个地方使用lazy evaluation：
 
 
  
   1.引用计数
  
 
 
  class string {};
  
  
 
 
  string s1 = "hello";
 
 
  string s2 = s1;
 
 
  s2=s1，这句应该调用string的拷贝构造函数，通过重新分配内存，拷贝字符串到s2实现。但是有可能s2根本不会被修改，只是用作输出，这样就可以使用lazy evaluatin，不用分配内存和拷贝字符串，而只要将s2指向s1同一个内存空间即可。
 
 
  
   2.区别对待读取和写入
  
 
 
  同上一个例子
 
 
  
   3.Lazy Fetching（懒惰提取）
  
 
 
  如一个类有很多成员，不一定要在构造函数中获取全部的成员变量的初值，尤其是需要I/O操作的成员，延后到使用时在读取不失为一个提高效率的解决方案。
 
 
  
   4.Lazy Expression Evaluation(懒惰表达式计算)
   
   
  
 


