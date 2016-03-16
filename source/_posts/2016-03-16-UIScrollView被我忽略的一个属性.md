---
layout: post
title: "UIScrollView被我忽略的一个属性"
date: 2016-03-16
comments: true
categories: [iOS]
tags: [iOS tips]
description: "iOS 经验总结"
keywords: iOS
---

### UIScrollView被我忽略的一个属性

> 最近在使用UIScrollView的时候，突然发现横屏滑动时没有按页滑动，停留的位置不满足要求。

刚开始觉得可能是scrollViewDidScroll里面计算页数不对，查了下发现不是这的问题。翻看头文件发现一个属性**pagingEnabled**，竟然跟page相关，难到是？试试设置为YES, 果然解决问题。

通过这个问题发现自己还是很多细节不够了解，一些属性和函数没有认真去研究过。之后需要主意下。
