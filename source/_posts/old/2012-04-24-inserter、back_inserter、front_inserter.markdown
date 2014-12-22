---
layout: post
title: "inserter、back_inserter、front_inserter"
date: 2012-04-24 15:17:00
comments: true
categories: [C++技术]
tags: [C++技术]
description: "inserter、back_inserter、front_inserter"
keywords: C++技术
---

inserter、back_inserter、front_inserter

分别返回三种类型的iterator：insert_iterator,back_insert_iterator,front_insert_iterator。这三种iterator被设计成允许不同的算法重写elements(例如copy方法)去替代插入操作。

```
// inserter example
#include <iostream>;
#include <iterator>;
#include <list>;
using namespace std;
int main () {
  list<int>; firstlist, secondlist;
  for (int i=1; i<=5; i++)
  { firstlist.push_back(i); secondlist.push_back(i*10); }
  list<int>;::iterator it;
  it = firstlist.begin(); advance (it,3);
  copy (secondlist.begin(),secondlist.end(),inserter(firstlist,it));
  for ( it = firstlist.begin(); it!= firstlist.end(); ++it )
      cout << *it << " ";
  cout << endl;
  return 0;
}

```
Output:
1 2 3 10 20 30 40 50 4 5

必须要提到的一点是，这三种迭代器是对容器有要求的，分别要求容器提供insert，push_back，push_front方法才行。
