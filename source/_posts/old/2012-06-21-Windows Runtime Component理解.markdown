---
layout: post
title: "Windows Runtime Component理解"
date: 2012-06-21 17:40:00
comments: true
categories: [C++技术]
tags: [C++技术]
description: "Windows Runtime Component理解"
keywords: C++技术
---

WinRT类型的动态库，可以认为导出的是类，应用通过创建类的实例来引用该导出类的功能即接口。

这里需要注意的是导出类的public成员只能是接口（成员变量是不可以为public），这样就避免了暴露类结构，并且接口中的参数不能为native type(例如：void* dd就不可以作为参数)。
