---
layout: post
title: "How to use Terminal in Vim"
date: 2013-04-10 21:14:00
comments: true
categories: [Vim]
tags: [Vim]
description: "How to use Terminal in Vim"
keywords: Vim
---

   这篇文章虽然叫做“如何在Vim中使用Terminal”，但是我这里要说的是在Windows下使用。

*  OS： Windows 8 64bit
*  Plugin：ConqueTerm.vim （该插件是利用socket与真正的terminal进行通信来操作terminal command的。插件是使用python写的）
*  Python：2.7
*  Vim：7.3

  在我的Linux下使用ConqueTerm一点问题都没有，非常好用，我的32位的Window7 上面也没有问题，只有在64位windows 8工作机上才会出现无法在vim中找到python接口。也就是说 echo has('python') = 0。通过查看vim的verison，发现已经打开了python/dyn。这个问题一连搞了一天也没有解决，果断放弃。这段时间vim的学习又有了突破，才又想起解决这个问题，通过大量的google search，终于在stackflow上面看到某人也有类似的问题，解决方法也是让我没有想到，重装vim或者python？为什么呢？原来是版本不匹配，我的机器安装的是64位的python，而vim确是32位的，果断下载一个64位的Vim，重新测试。OK，一切正常！

  通过这个问题，我的收获就是当一个问题陷入死胡同的时候，不应该去放弃；可以暂时先放下，调整下心情状态，或者过段时间再来解决，很有可能就会柳暗花明又一村！
