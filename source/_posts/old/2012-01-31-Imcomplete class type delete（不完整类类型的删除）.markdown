---
layout: post
title: "Imcomplete class type delete（不完整类类型的删除）"
date: 2012-01-31 20:51:00
comments: true
categories: [C++技术]
tags: [C++技术]
description: "Imcomplete class type delete（不完整类类型的删除）"
keywords: C++技术
---

当delete不完整类类型对象的时候，不会调用对象的析构函数，导致内存泄漏。所以要避免delete 不确定类型。

首先解释下不完整类类型Imcomplete class type：只见声明不见定义的类、结构体或是联合体；相对应的就是complete type，就是编译器可以确定的类型。

下面举例说明：

```
//delete_object.h
class CObject;
void delete_object(CObject* pObj); //p为imcomplete type
//delete_object.cpp
void delete_object(CObject* pObj) {delete pObj;}
//CObject.h
class CObject
{
    //destructor
    ~CObject()
    {
        //destructor called.
    }
}
//main file
#include "delete_object.h"
#include "CObject.h"
int main(int argc, char[] argv)
{
    CObject* pObj = new CObject();
    delete_object(pObj);
}

```

这里的pObj对象在delete的时候就是不确定对象，编译器不知道它的类型，无法调用析构函数，最终导致内存泄漏。解决的最简单的方法，就是在delete_object.cpp文件中增加#include “CObject.h”语句即可。

我们这里要重点介绍的是boost库中的使用的方法，方法使用的非常巧妙。整理如下：
```
//utiles.h
template<typename T>;
inline void checked_delete(T * x)
{
    // intentionally complex - simplification causes regressions
    typedef char type_must_be_complete[ sizeof(T)? 1: -1 ];
    (void) sizeof(type_must_be_complete);
    delete x;
}
template<typename T>;
inline void checked_arraydelete(T * x)
{
    // intentionally complex - simplification causes regressions
    typedef char type_must_be_complete[ sizeof(T)? 1: -1 ];
    (void) sizeof(type_must_be_complete);
    delete[] x;
}
template<typename T>;
struct checked_deleter
{
    typedef void result_type;
    typedef T * argument_type;
    void operator()(T * p) const
    {
        checked_delete(p);
    }
};
template<typename T>;
struct checked_arraydeleter
{
    typedef void result_type;
    typedef T * argument_type;
    void operator()(T * p) const
    {
        checked_arraydelete(p);
    }
};
//main file
//#include "delete_object.h"
#include "CObject.h"
#include "utiles.h"
int main(int argc, char[] argv)
{
    CObject* pObj = new CObject();
    //delete_object(pObj);
    checked_deleter<CObject>;(p);
}
```

既能在静态编译时监测出delete是否有问题，也可以安全的删除，不用再担心内存泄漏了！
