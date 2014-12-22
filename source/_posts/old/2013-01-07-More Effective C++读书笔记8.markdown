---
layout: post
title: "More Effective C++读书笔记8"
date: 2013-01-07 11:10:00
comments: true
categories: [C++技术]
tags: [C++技术]
description: "More Effective C++读书笔记8"
keywords: C++技术
---

## Item 8：理解各种不同含义的new和delete

new操作符在C++中做了两件事，1是在堆上分配足够的内存，2是调用类的构造函数。new操作法的这种行为是程序员无法改变的，唯一程序员可以去做的就是按自己的需求来修改内存分配，通过重载operator new函数（void* operator new（size_t size））来自定义内存的分配，但是在调用构造函数上面是由编译器来决定的。如果要重载自己的operator new函数，第一个参数必须为size_t。

    例如：
```
Obeject* obj = new Object（）；
```

被编译器解析：

1.allocate memory
2.call Object（）构造函数

关于placement new，就是在已经分配的内存上创建对象，例如：
```
void* placementnewSample（void* buffer， size_t）
{
    return new （buffer） Object();
}
```
注意，如果是调用的placement new，则对应的一定不要调用delete操作符，因为该对象是在buffer中分配的。可以直接调用对象的析构函数。数组的new操作符表现形式不同单个对象的new，但是做的事情是一样的。只不过是需要将所有数组元素都分配内存并依次调用构造函数。

```
void* operator new[](size_t size)；
```

为new数组分配内存函数，可以被重载。delete操作符作为new操作符对应的相反行为，做的事情自然跟new相反，1调用析构函数，2释放已经分配的内存。如果你重载了operator new，则你必须重载operator delete来对应，否则有可能导致new和delete行为的不一致性。什么样的new对应什么样的delete，即单个对象的new对应单个对象的delete，new[]对应delete[]。注意，尽量不要重载全局的new和delete，这样会造成程序的不兼容性，除非这是你想要的结果。
