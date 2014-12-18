---
layout: post
title: "std::vector 两种操作的比较"
date: 2013-07-06 17:27:00 
comments: true
categories: [C++技术]
tags: [C++技术]
description: "std::vector 两种操作的比较"
keywords: C++技术
---


 
  
   swap
  
  
   assign
  
 
 
  这里只想说明这三种操作的用处和效率。swap和assign都可以用在将一个vector的内容全部复制给另外一个vector，区别是swap会改变源vector，而assign会清空目的vector后再将源vector的值全部插入到目的vector中。就效率而言，swap只是交换vector的头指针，时间复杂度是常数；而assigin时间复杂度则是线性。
 
 
  
  
 
 
  #include <vector>;
#include "DebugUtility.h"
#include <iostream>;
#include <algorithm>;
#include <string>;

using namespace std;

void print(int x)
{
    cout << x << endl;
}

void Swap(vector<string>;& source, vector<string>;& dest)
{
    DebugUtility temp;
    dest.swap(source);
}

void Assign(vector<string>;& source, vector<string>;& dest)
{
    DebugUtility temp; 
    dest.assign(source.begin(), source.end());
}

int main(int argc, const char *argv[])
{
    vector<string>; source(900000, "90");
    vector<string>; destination(1, "abc");

    Swap(source, destination);
    //source.clear();
    //for_each(destination.begin(), destination.end(), print);

    //Assign(source, destination);
    //source.clear();
    //for_each(destination.begin(), destination.end(), print);

    return 0;
}
  
   结果：
  
 
 
  Total time elapsed : 1 us
900000

Total time elapsed : 12391 us
900000
  
   DebugUtility.h  大家可以从https://github.com/TimBao/Material.git/Utility取得。
  
 


