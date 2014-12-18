---
layout: post
title: "More Effective C++读书笔记18"
date: 2013-01-19 16:29:00 
comments: true
categories: [C++技术]
tags: [C++技术]
description: "More Effective C++读书笔记18"
keywords: C++技术
---


 
  
   Item 20：协助完成返回值优化
  
 
 
  返回对象时的开销会比较大，会调用对象的构造和析构函数，但是当一个函数必须要返回对象时，这种构造和析构造成的开销是无法消除的。那么还能优化么？
  
   以某种方法返回对象，能让编译器消除临时对象的开销，这样编写函数通常是很普遍的。这种技巧是返回constructor argument而不是直接返回对象。你可以这样做：
   
    const Rational operator*(const Rational& lhs, const Rational& rhs)
{
    return Rational(lhs.numerator() * rhs.numerator(),
                  lhs.denominator() * rhs.denominator());
}
    仔细观察被返回的表达式。它看上去好象正在调用Rational的构造函数，实际上确是这样。你通过这个表达式建立一个临时的Rational对象，
    
     Rational(lhs.numerator() * rhs.numerator(),  lhs.denominator() * rhs.denominator());
     
      并且这是一个临时对象，函数把它拷贝给函数的返回值。
      
       返回constructor argument而不出现局部对象，这种方法还会给你带来很多开销，因为你仍旧必须为在函数内临时对象的构造和释放而付出代价，你仍旧必须为函数返回对象的构造和释放而付出代价。但是你已经获得了好处。C++规则允许编译器优化不出现的临时对象（temporary objects out of existence）。因此如果你在如下的环境里调用operator*：
       
        Rational a = 10;
        
         Rational b(1, 2);
         
          Rational c = a * b;                          // 在这里调用operator*
          
           编译器就会被允许消除在operator*内的临时变量和operator*返回的临时变量。它们能在为目标c分配的内存里构造return表达式定义的对象。如果你的编译器这样去做，调用operator*的临时对象的开销就是零：没有建立临时对象。你的代价就是调用一个构造函数――建立c时调用的构造函数。而且你不能比这做得更好了，因为c是命名对象，命名对象不能被消除（参见条款M22）。不过你还可以通过把函数声明为inline来消除operator*的调用开销。
           
           
          
         
        
       
      
     
    
   
  
 
 
  
  
 
 
  看完这篇突然认识到，理解编译器优化是非常重要的，对编译器了解构思，代码的效率自然有保证。路漫漫其修远兮，吾将上下而求索！
 


