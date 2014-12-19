---
layout: post
title: "将win32的lib库直接转为winRT静态库"
date: 2012-07-04 14:38:00 
comments: true
categories: [Windows 8]
tags: [Windows 8]
description: "将win32的lib库直接转为winRT静态库"
keywords: Windows 8
---

 将win32的lib库直接转为winRT静态库需要注意三点
  1、打开/ZW开关，关闭/GM开关
   2、忽略OLE32.LIB
    3、手动使用命令/AI包含Windows.winmd(从工程中add new references无效)
      /FU Platform.winmd /FU Windows.winmd /AI "$(VCInstallDir)vcpackages" /AI "$(WindowsSDK_MetadataPath)"
      剩余的工作就是修改编译错误了
