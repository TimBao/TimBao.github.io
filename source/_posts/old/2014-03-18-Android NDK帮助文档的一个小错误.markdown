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

> 刚刚在调试如何不安装`Cgywin`的情况下，利用`NDK`编译`cocos2d-x`的SampleGame.却机缘巧合的情况下发现了`android-ndk-r8d`的*docs/IMPORT-MODULE.html*下的一个小bug。

  在Eclipse中导入工程后，工程目录如下：
  ![image](http://img.blog.csdn.net/20140318204030468?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZ29vZg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

 * 将工程转换为C++工程。这步不会的可以google下。

 * 设置ndk目录。
  ![image](http://img.blog.csdn.net/20140318204205890?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZ29vZg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
 * 设置`NDK_MODULE_PATH`环境变量。
  ![iamge](http://img.blog.csdn.net/20140318204205890?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZ29vZg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

 在docs/IMPORT-MODULE.html中的介绍：
> I.NDK_MODULE_PATH: The NDK_MODULE_PATH variable must contain a list of directories. Due to GNU Make limitations, NDK_MODULE_PATH must not contain any space. The NDK will complain if this is not the case. Use ':' as the path separator. On Windows, use '/' as the directory separator.

 这里面说的Use ':' as the path separator, 经过实践验证在windows平台路径应该是使用;分割.
