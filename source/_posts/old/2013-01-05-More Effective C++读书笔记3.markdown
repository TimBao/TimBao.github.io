---
layout: post
title: "More Effective C++读书笔记3"
date: 2013-01-05 09:42:00 
comments: true
categories: [C++技术]
tags: [C++技术]
description: "More Effective C++读书笔记3"
keywords: C++技术
---

  Item 3: 不要对数组使用多态
   语言规范中说通过一个基类指针来删除一个含有派生类对象的数组，结果将是不确定的。
     class BST { ... };
class BalancedBST: public BST { ... };
void printBSTArray(ostream& s,const BST array[], int numElements)
{
    for (int i = 0; i < numElements; )
    {
        s << array[i]; //假设 BST 类重载了操作符<<
    }
} 
        编译器原先已经假设数组中元素与 BST 对象的大小一致，但是现在数组中每一个对象大小却与 BalancedBST 一致。派生类的长度通常都比基类要长。我们料想 BalancedBST 对象长度的比 BST 长。如果如此的话，printBSTArray 函数生成的指针算法将是错误的，没有人知道如果用 BalancedBST 数组来执行 printBSTArray 函数将会发生什么样的后果。不论是什么后果都是令人不愉快的。
        值得注意的是如果你不从一个具体类（concrete classes） （例如 BST）派生出另一个具体类（例如 BalancedBST），那么你就不太可能犯这种使用多态性数组的错误。
