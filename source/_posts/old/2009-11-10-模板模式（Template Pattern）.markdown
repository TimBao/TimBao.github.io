---
layout: post
title: "模板模式（Template Pattern）"
date: 2009-11-10 20:15:00
comments: true
categories: [设计模式学习]
tags: [设计模式学习]
description: "模板模式（Template Pattern）"
keywords: 设计模式学习
---

模板(Template)模式：

父类将算法封装，子类提供算法的一个或全部的实现。

解决的问题：

对于某一业务逻辑（算法实现）在不同的对象中有不同的细节实现，但是逻辑（算法）的框架（或通用的应用算法），模板模式提供了这种情况的一种实现框架。

Template模式获得一种反向控制结构效果，这也是面向对象系统分析和设计中一个原则：DIP(依赖倒置：Dependency Inversion Principles).

其含义就是父类调用子类的操作(高层模块调用底层模块的操作)，底层模块实现高层模块声明的接口。这样控制权在父类(高层模块)，底层模块反而要依赖高层模块。

参考：《设计模块精解》
