---
layout: post
title: "VS调试-library版本引起的内存问题"
date: 2012-04-23 14:50:00
comments: true
categories: [C++技术]
tags: [C++技术]
description: "VS调试-library版本引起的内存问题"
keywords: C++技术
---

VS建立的项目，其中肯定涉及到DLL或者是LIB库，这时候调试起来会有一定不便，这里建议大家将相关项目建立成一个solution，只要设置好路径，就可以调试了，而且调试起来非常方便。但是当lib库或DLL库中涉及到内存分配，但是释放却在其他库或上层软件时，切记要保证EXE或DLL或者LIB的版本要一致，这里说的一直就是要求必须都是debug版本或者release版本，否则在内存处理上会出现一些问题。

例如：

在DLL中new出的内存，在上层delete的时候，会由于版本不一致，导致vs在对内存进行检查的时候略有不同，导致程序崩溃。希望大家多注意下。
