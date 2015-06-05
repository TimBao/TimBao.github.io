---
layout: post
title: "2015-06-05 Use NSTimer attention"
date: 2015-06-05
comments: true
categories: [iOS]
tags: [iOS tips]
description: "iOS 经验总结"
keywords: iOS
---

### NSTimer 在使用中需要注意的几个问题：

* NSTimer在UITableViewCell中使用是，需要将timer加到runLoop中。

    ```objc

    countDownTimer = [NSTimer timerWithTimeInterval:1.0 target:self selector:@selector(countDown:) userInfo:nil repeats:YES];
    NSRunLoop *currentRunLoop = [NSRunLoop currentRunLoop];
    [currentRunLoop addTimer:countDownTimer forMode:NSRunLoopCommonModes];

    ```

* 同一个timer在重复使用之前必需invalidate, 否则会造成之前的timer无法停掉，两个timer同时存在。导致的现象就是timer同时更新两次。
