---
layout: post
title: "delete对象后到底要不要将对象置为NULL"
date: 2012-11-22 14:31:00
comments: true
categories: [C++技术]
tags: [C++技术]
description: "delete对象后到底要不要将对象置为NULL"
keywords: C++技术
---

以下是我的一个美国同事丹尼尔对此的看法：

>  Member pointers do NOT have to be set to NULL in a destructor. A destructor, as its name implies, destroys an object and the object can’t be accessed after that. So accessing anything (even if it were a pointer set to NULL) is absolutely wrong. I usually take setting pointers to NULL out of destructors because it is not needed. If an object gets accessed after it is destroyed, in any form, than it has to be tracked down. But NOT by accessing any of its member variables.

 > 对象指针不需要设置为NULL在析构函数中。析构函数，顾名思义，销毁一个对象而之后该对象不在被使用到。所以存取任何东西（即使指针被设置为NULL）都是绝对的错误。我一般都将设置指针为NULL的语句从析构函数中移除，因为它并不被需要。如果一个对象能够在它被销毁后存取，那么这将能被追查出。但是不是因为存取它里面的任何成员。

对此他还写了个小程序来证明他的观点：
```
class A
{
public:
    // Constructor
    A()
    {
        m_pMember = new int;
    }
    // Destructor
    ~A()
    {
        delete m_pMember;
        // UNNECESSARY: You can't access that pointer after the destructor!
        m_pMember = NULL;
    } int* m_pMember;
};
int _tmain(int argc, _TCHAR* argv[])
{
    // Instantiate
    A* pA = new A;
    // Destroy, destructor gets called!
    delete pA;
    // THIS IS WRONG, you can't access m_pMember anymore. You can't even check if for NULL!
    // This probably won't crash in 99%  of the cases, but it can crash!
    if (pA->;m_pMember)
    {
        // Do something
    }
      return 0;
}
```

看起来他的程序的确能证明他的观点。但是他忘记了一点，关于对象的copying。如果对象pA在delete之前copying给了其他对象，那么结果可是大不一样啊！让我们稍微改动下程序再看看效果:
```
class A
{
public:
    A() { m_pMember = new int;}
    ~A() {
        delete m_pMember;
    }
    int* m_pMember;
};
int _tmain(int argc, _TCHAR* argv[])
{
    A* pA = new A();
    A* pB = pA;
    delete pA;
    if (pB->;m_pMember)
    {
        int temp = *pB->;m_pMember; //Crash
    }
    return 0;
}
```

这还只是个简单的情况，如果涉及到多线程或者多模块如多个dll之间传递对象，那么就极有可能因为某一对象已经销毁后，没有设置为NULL导致程序Crash。在这里还是建议各位一定要谨慎处理指针问题，避免引起不必要的奇怪问题。
