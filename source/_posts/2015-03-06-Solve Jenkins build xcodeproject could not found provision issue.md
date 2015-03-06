---
layout: post
title: "2015-03-06 Solve jenkins build xcode project could not found provision issue"
date: 2015-03-06
comments: true
categories: [iOS]
tags: [iOS Jenkins CI]
description: ""
keywords: iOS Jenkins CI
---

The error is:
Code Sign error: No matching provisioning profile found

How to solve:

1-Ensure the project is building successfully from Xcode to real target.

2-Copy all the development cretificates & credentials form your user folder to the system folder in KeyChain.

3-Copy all the Provisioning profiles existing in Users/<user>/Library/MobileDevice/Provisioning Profiles to /System/Library/MobileDevice/Provisioning Profiles.

If the directory is not exist, please create first.


参考：
https://issues.jenkins-ci.org/browse/JENKINS-20916
