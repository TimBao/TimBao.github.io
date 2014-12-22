---
layout: post
title: "NSMutableArray使用中遇到的内存问题"
date: 2012-03-15 15:58:00
comments: true
categories: [Iphone/Objective-C]
tags: [Iphone/Objective-C]
description: "NSMutableArray使用中遇到的内存问题"
keywords: Iphone/Objective-C
---

```
NSMutableArray* array1 = [[NSMutableArray alloc] init];
NSMutableArray* array2 = [[NSMutableArray alloc] init];
- (void)pushs:(NSInteger)VALUE {
    BOOL find = NO;
    [array2 removeAllObjects];
    for (NSNumber* item in array1) {
        NSInteger value = [item intValue];
        if (value == VALUE) {
            [array1 removeAllObjects];
            [array1 addObjectsFromArray: array2];
            [array1 addObject: item];
            find = YES;
            break;
        }
        else {
            [array2 addObject: item];
        }
    }
    if (!find) {
        NSNumber* newItem = [NSNumber numberWithInt: VALUE];
        [viewTypeArray addObject:newItem];
    }
}
```

当进入if (value == VALUE)后，[array1 addObject: value];执行完毕后，查看array1中的元素类型发现已经不再是NSNumber*而是变为NSobject*,取出使用报错，bad access。

该问题出现的几率比较随机。单独使用没有任何问题，放到整个工程中就出现问题。通过查看内存，发现NSMutableArray中存储的为NSNumber指针，array1和array2中指针地址一致，有可能是当[array1 removeAllObjects];后，item有可能被自动销毁，导致加入到array1中的值出现错误。修改为：

```
NSNumber* topItem = [NSNumber numberWithInt: value];
[array1 addObject:topItem];
```
问题解决。这个问题的根本原因还是内存的问题，objective-c的内存管理比较特殊，有很多隐藏的内存分配和自动回收，个人怀疑numberWithInt方法返回的NSNumber*对象为autorelease。
