---
layout: post
title: "使用List：：unique()代替循环判断"
date: 2012-11-26 16:36:00
comments: true
categories: [记事]
tags: [记事]
description: "使用List：：unique()代替循环判断"
keywords: 记事
---

今天很兴奋，帮助同事改进了程序的算法，将Filter时间提高了将近5倍。

原来的程序采用循环对list进行判断，防止重复push数据，而且是循环嵌套循环，时间复杂度0(n^2)，而改用list::sort()和list::unique()后，效率得到了很大的提高。

看来《STL源码分析》木有白看啊！
