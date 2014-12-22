---
layout: post
title: "strcmp, stricmp — compare strings."
date: 2012-03-06 13:33:00
comments: true
categories: [C++技术]
tags: [C++技术]
description: "strcmp, stricmp — compare strings."
keywords: C++技术
---

Returns

A negative number if the first string is ordinally less than the second string according to the ASCII character set, a positive number if the first string is greater than the second, and zero if the two strings are exactly equal.

Description

These functions can be used as comparison functions for sort and insert.

stricmp performs the same function as strcmp, but alphabetic characters are compared without regard to case. That is, "A" and "a" are considered equal by stricmp, but different by strcmp.

简单来说，strcmp区分大小写而stricmp不区分大小写而已。
