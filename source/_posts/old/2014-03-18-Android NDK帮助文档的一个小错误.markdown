---
layout: post
title: "Android NDK帮助文档的一个小错误"
date: 2014-03-18 20:26:00 
comments: true
categories: [Android NDK]
tags: [Android NDK]
description: "Android NDK帮助文档的一个小错误"
keywords: Android NDK
---

   刚刚在调试如何不安装
    Cgywin
   的情况下，利用
    NDK
   编译
    cocos2d-x
   的
    SampleGame
   .却机缘巧合的情况下发现了
    android-ndk-r8d
   的
    docs/IMPORT-MODULE.html
   下的一个小bug。
  在
   Eclipse
  中导入工程后，工程目录如下：
   将工程转换为C++工程。这步不会的可以google下。
   设置ndk目录。
   设置
    NDK_MODULE_PATH
   环境变量。
  在
   docs/IMPORT-MODULE.html
  中的介绍：
   I.
    NDK_MODULE_PATH
   :
   The
    NDK_MODULE_PATH
   variable
 must contain a list of directories.
     Due to GNU Make limitations,
      NDK_MODULE_PATH
     must
 not contain any space. The NDK will complain if this is not the case.
       Use
 ':' as the path separator
     .
     On Windows, use '/' as the directory separator.
  这里面说的
   Use
 ':' as the path separator
  ,经过实践验证在windows平台路径使用
   ;
  分割
