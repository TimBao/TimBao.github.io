---
layout: post
title: "More Effective C++读书笔记9"
date: 2013-01-08 11:54:00 
comments: true
categories: [C++技术]
tags: [C++技术]
description: "More Effective C++读书笔记9"
keywords: C++技术
---

  Item 9：使用析构函数防止资源泄漏
   这一章我觉得可以算作是异常安全处理的一种情况，也就是说在异常发生的情况下，保证资源能被正确释放。
    这里分为两种情况来讨论，
     一是指针操作
      举个例子：
       void myFucntion()
{
    Object* obj = getBack();
    obj->;doSomething();
    delete obj；
}
         如果在obj->;doSomething()中发生异常，那么delete obj将不会执行，这是个很显然的泄露问题。想到的第一解决方法基本都是加上try...catch（），Ok那么让我们重新整理代码。
          void myFucntion()
{
    Object* obj = getBack();
    try
    {  
        obj->;doSomething();
    }
    catch(...)
    {
        delete obj；
    }
    delete obj；
}
            好吧，我承认，程序很丑陋，代码重复，那么继续改进。
             void myFucntion()
{
    Object* obj = getBack();
    try
    {  
        obj->;doSomething();
    }
    catch(...)
    {
        ...
    }
    finally
    {
        delete obj；
    }
}
               Ok，问题已经解决。但是还不要高兴，满篇的try catch，如果有很多地方都需要加try catch，那么工作量也是非常惊人的，当然你也可以这样写，但是我觉得作为程序员就是要有“懒惰”的思想，要简化，越简单越好，代码越少出错的地方也就越少。那么你一定想到了，利用智能指针auto_ptr或者share_ptr来解决问题。Ok，尽管不推荐auto_ptr，但是老的代码中还是会出现的。
                void myFucntion()
{
    share_ptr<Object*>; obj(getBack());
    obj->;doSomething();
}
                  Perfect!
                    二是对象操作
                     最常见的问题时windows中的各种handler的关闭泄露问题，我们可以针对handler进行一个封装，在析构函数中释放handler。
                      class WindowHandle
{
public:
    WindowHandle(WINDOW_HANDLE handle): w(handle) {}
    ~WindowHandle() { destroyWindow(w); }
    operator WINDOW_HANDLE() { return w; } // see below
private:
    WINDOW_HANDLE w;
    WindowHandle(const WindowHandle&);
    WindowHandle& operator=(const WindowHandle&);
}; 
