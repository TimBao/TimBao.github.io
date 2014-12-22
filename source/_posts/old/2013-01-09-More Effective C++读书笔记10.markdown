---
layout: post
title: "More Effective C++读书笔记10"
date: 2013-01-09 11:49:00
comments: true
categories: [C++技术]
tags: [C++技术]
description: "More Effective C++读书笔记10"
keywords: C++技术
---

## Item 10：在构造函数中防止资源泄漏

该条款讲述的内容跟上一条款很相似，应该都可以归属为编写异常安全代码范畴。在构造函数中尽量处理简单的代码，防止抛异常，因为C++并没有对构造函数异常做析构的动作，并不会在构造函数发生异常的情况下，调用析构函数去释放资源。这是考虑到效率问题。

我们还是举个例子更直观的描述如何在构造函数中编写异常安全的代码，要做个通讯录类，内容包括姓名，地址，图片和一段声音。
```
class Image
{
public:
    Image(string name);
private:
    string imageName;
}；
class Audio
{
public:
    Audio(string name);
private:
    string audioName;
};
class BookEntry
{
public:
    BookEntry(string name, string adress, Image* image, Audio* audio);
    ~BookEntry();
private:
    string m_name;
    string m_adress;
    Image* m_image;
    Audio* m_audio;
};
BookEntry::BookEntry(string name, string adress, string imageName, string audioName)
: m_name(name), m_adress(adress),m_image(NULL),m_audio(NULL)
{
   m_image = new Image(imageName);
   m_audio = new Audio(audioName);
}
BookEntry::~BookEntry()
{
    delete m_image;
    delete m_audio;
}
```

因为C++ delete删除空指针是不做任何操作的，所以析构函数中并没有对指针进行判断。

我们考虑正常情况下构造函数全部指向完毕（完全构造），当销毁BookEntry的时候，会调用析构函数释放资源，没有任何问题。但是如果是构造函数中出现异常情况怎么办？最简单的办法就是万能的try catch。

```
BookEntry::BookEntry(string name, string adress, string imageName, string audioName)
: m_name(name), m_adress(adress),m_image(NULL),m_audio(NULL)
{
    try
    {
        m_image = new Image(imageName);
        m_audio = new Audio(audioName);
    }
    catch(...)
    {
        delete m_image；
        delete m_audio;
    }
}
```

OK, 没有问题了，但是看起来catch中的代码跟析构函数的代码一样，OK，利用refactoring method我们可以合并代码，abstract a public method。

那么如果我们再修改下代码，将成员变量改为const常量
```
Image* const m_image;
Audio* const m_audio;
```
那么就只能在成员初始化列表中对指针进行初始化了。
```
BookEntry::BookEntry(string name, string adress, string imageName, string audioName)
: m_name(name), m_adress(adress),m_image((iamgeName.empty()?NULL:new Image(imageName))),m_audio((audioName.empty()?NULL:new Audio(audioName)))
{}
```
在成员初始化函数列表中，是不可以包含语句的，只能使用表达式，那么try catch就无法再使用了，那么该如何解决呢？估计看过前一章的一定会想到的，对就是使用智能指针。
```
const share_ptr<Image>; m_image;
const share_ptr<Audio>; m_audio;
BookEntry::BookEntry(string name, string adress, string imageName, string audioName)
: m_name(name), m_adress(adress),m_image((iamgeName.empty()?NULL:new Image(imageName))),m_audio((audioName.empty()?NULL:new Audio(audioName)))
{
}
BookEntry::~BookEntry()
{
}
```
连析构函数也省了。
