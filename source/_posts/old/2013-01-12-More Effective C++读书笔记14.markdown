---
layout: post
title: "More Effective C++读书笔记14"
date: 2013-01-12 11:39:00
comments: true
categories: [C++技术]
tags: [C++技术]
description: "More Effective C++读书笔记14"
keywords: C++技术
---

## Item 14：审慎使用异常规格(exception specifications)

  何为异常规格，通俗的理解就是对异常的规范的说明。它明确地描述了一个函数可以抛出什么样的异常。但是它不只是一个有趣的注释。编译器在编译时有时能够检测到异常规格的不一致。而且如果一个函数抛出一个不在异常规格范围里的异常，系统在运行时能够检测出这个错误，然后一个特殊函数unexpected将被自动地调用。异常规格既可以做为一个指导性文档同时也是异常使用的强制约束机制。

  不过在通常情况下，美貌只是一层皮，外表的美丽并不代表其内在的素质。函数unexpected缺省的行为是调用函数terminate，而terminate缺省的行为是调用函数abort，所以一个违反异常规格的程序其缺省的行为就是halt（停止运行）。在激活的栈中的局部变量没有被释放，因为abort在关闭程序时不进行这样的清除操作。对异常规格的触犯变成了一场并不应该发生的灾难。

  一个函数调用了另一个函数，并且后者可能抛出一个违反前者异常规格的异常，（A函数调用B函数，但因为B函数可能抛出一个不在A函数异常规格之内的异常，所以这个函数调用就违反了A函数的异常规格  译者注）编译器不对此种情况进行检测，并且语言标准也禁止编译器拒绝这种调用方式（尽管可以显示警告信息）。

  虽然防止抛出unexpected异常是不现实的，但是C++允许你用其它不同的异常类型替换unexpected异常，你能够利用这个特性。例如你希望所有的unexpected异常都被替换为UnexpectedException对象。你能这样编写代码：
```
class UnexpectedException {};          // 所有的unexpected异常对象被
                                       //替换为这种类型对象
void convertUnexpected()               // 如果一个unexpected异常被
{                                      // 抛出，这个函数被调用
    throw UnexpectedException();
}
set_unexpected(convertUnexpected);
```

  通过用convertUnexpected函数替换缺省的unexpected函数，来使上述代码开始运行。
  下面看另外一个书中例子：
```
class Session {
public:
    ~Session();
  ...
private:
    static void logDestruction(Session *objAddr) throw();
};
Session::~Session()
{
   try
   {
       logDestruction(this);
   }
   catch (...) {  }
}
```

  session的析构函数调用logDestruction记录有关session对象被释放的信息，它明确地要捕获从logDestruction抛出的所有异常。但是logDestruction的异常规格表示其不抛出任何异常。现在假设被logDestruction调用的函数抛出了一个异常，而logDestruction没有捕获。我们不会期望发生这样的事情，但正如我们所见，很容易就会写出违反异常规格的代码。当这个异常通过logDestruction传递出来，unexpected将被调用，缺省情况下将导致程序终止执行。这是一个正确的行为，但这是session析构函数的作者所希望的行为么？作者想处理所有可能的异常，所以好像不应该不给session析构函数里的catch块执行的机会就终止程序。如果logDestruction没有异常规格，这种事情就不会发生（一种防止的方法是如上所描述的那样替换unexpected）。
