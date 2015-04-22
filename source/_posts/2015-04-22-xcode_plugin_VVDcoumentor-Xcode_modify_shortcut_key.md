---
layout: post
title: "2015-04-22 VVDocuemnter-Xcode plugin could not work correctly"
date: 2015-04-22
comments: true
categories: [iOS]
tags: [xcode plugin]
description: "VVDocuemnter-Xcode plugin do not work"
keywords: iOS
---

> `VVDocuemnter-Xcode` plugin 是xcode上非常好用的一个插件，尤其适合做sdk接口的lazy programer。最为一个标准的`懒人`，必备之神器之一。但是当我安装完后却发现无法使用。怪哉，奇哉！

google 了一圈下来，没有任何收获。索性自己看看源码，再不济也可以调试下。（开源的好处在这时候体现的淋漓尽致.）

* 运行自带Test, 没有问题。

* 打印Log，发现生成comments正常。

* 调试，终于发现了问题，下面三行代码，注意看comments **Cmd+V, paste**, 看到这里我突然想起来，我把paste的快捷键改成control＋V了，再往下看keyCode支持`command`, `alt`, `shift`, `control`四种组合键，默认为`command`， 修改为我自定义的`control`后，重新安装，重启xcode，果然okay。

```Objective-C
//Cmd+V, paste (If it is Dvorak layout, use '.', which is corresponding the key 'V' in a QWERTY layout)
NSInteger kKeyVCode = [[VVDocumenterSetting defaultSetting] useDvorakLayout] ? kVK_ANSI_Period : kVK_ANSI_V;
[kes sendKeyCode:kKeyVCode withModifierCommand:NO alt:NO shift:NO control:YES];
```

> 通过解决这个插件的问题，我知道了plugin的结构，了解了如何调试plugin，最终也解决了自己的问题。所获丰富，有时候问题是使自身进步最好的导火线！珍惜错误把！
