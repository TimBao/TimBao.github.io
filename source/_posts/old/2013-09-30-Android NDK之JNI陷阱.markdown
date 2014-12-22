---
layout: post
title: "Android NDK之JNI陷阱"
date: 2013-09-30 21:54:00
comments: true
categories: [C++技术]
tags: [C++技术]
description: "Android NDK之JNI陷阱"
keywords: C++技术
---

> 背景： 最近一个月一直在做移植库的工作，将c代码到share library移植到Android平台。这就涉及到Android NDK(native develop kit)内容。这里只想记录下JNI(java native interface)经常遇到到问题。

  问题1.  忘记delete local reference。带New到方法(如：NewByteArray)这样到方法比较好辨认，需要手动调用DeleteLocalRef()来释放(返回值除外)。比较特殊的一个方法是：GetByteArrayELement必须要调用ReleaseByteArrayElements进行释放。当然如果你只是取bytearray中到byte，那么完全可以用GetByteArrayRegion实现。

  问题2. 没有NewGlobalRef。 在不同线程调用java方法，需要保存jobject对象，这时需要对jobject对象做全局引用，否则会失效。

  问题3.  jbytearray的length。在JNI layer获取到jbytearray到长度是不对到，应该由java获取byte[]的length再传给C layer。否则C layer有可能获得到是乱码。

  问题4.  线程问题。 不同线程使用JNIEnv*对象，需要AttachCurrentThread将env挂到当前线程，否则无法使用env。

  问题5.  javap 命令是对java的class文件操作；而javah命令需要在包名到上一层路径运行才行，否则无法生成.h文件。

  问题6. 尽量避免频繁调用JNI或者是使用JNI传输大量到数据。

  问题7. Reference Table overflow (max=1024) 或者是 Reference Table overflow (max=512)一定是因为忘记释放global reference或者local reference，请仔细检查代码。

  问题8. 不要在windows下使用cygwin编译NDK code，那样会遇到arguments too long问题，因为windows路径长度有限制导致。虽然可以使用subst将路径映射为短路径，但是在编译时间和调试上，windows到孩子都是伤不起。同样到build，在windows下要15分钟左右，而在mac下只要5分多，相差3倍。调试JNI 代码到速度更是不用提了，差太多。

  *总结*，JNI代码量其实不是很多，JNI作为一个数据传输层，它到作用仅仅是java和c直接到桥梁，但是如果处理不好将会是灾难，调试和找bug非常困难。
