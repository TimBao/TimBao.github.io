---
layout: post
title: "More Effective C++读书笔记11"
date: 2013-01-10 10:54:00 
comments: true
categories: [C++技术]
tags: [C++技术]
description: "More Effective C++读书笔记11"
keywords: C++技术
---

  Item 11：禁止异常信息（exceptions）传递到析构函数外
   书中所言，会有两种情况调用析构函数。第一种是在正常情况下删除一个对象，例如对象超出作用域或者显示delete。第二种是异常传递的堆栈辗转开解（stack-unwinding）过程中，由异常处理系统删除一个对象。在一个异常被激活的同时，析构函数也抛出异常，并导致程序控制权转移到析构函数外，C++将调用 terminate 函数。
     为什么要防止在析构函数中的异常传递到函数外，除了上面说的第二种情况外，还有就是将异常传递出去后，程序转到异常处理程序，析构函数中异常之后的程序将不会再被执行，这有可能会导致资源的泄露。
       这里对stack unwinding说明下，堆栈展开是C++的一个概念，每个函数都回有它的堆栈，调用函数时会对函数参数和局部成员进行入栈，而函数结束时会按入栈相反的顺序进行出栈。举个例子说下的上面的第二种情况：
        void func（string s）
{
    object obj（s）；
    obj.dosomething();
}
        如果在dosomething()中发生某些异常，那么异常将传递出去，直到遇到异常捕获函数为止。而随着异常的传递，函数func已经结束，其局部变量obj会被析构，如果在析构的过程中再次出现异常，那么C++将调用 terminate 函数，程序直接终止。
