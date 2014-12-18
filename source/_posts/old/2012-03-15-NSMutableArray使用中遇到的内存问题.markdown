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
  
   
   
  
 
 
  
   当
  
  
   进
  
  
   入
  
  if (value == VALUE)
  
   后，
  
  [array1
 addObject: value];
  
   执行
  
  
   完
  
  
   毕
  
  
   后，
  
  
   查
  
  
   看array1中的元素类型
  
  
   发现
  
  
   已
  
  
   经
  
  
   不再
  
  
   是
  
  NSNumber*
  
   而是
  
  
   变为
  
  NSobject*
  
   ,
 取出使用
  
  
   报错
  
  
   ，bad access。
  
  
   该问题
  
  
   出
  
  
   现
  
  
   的
  
  
   几率比较随机。单独使用没有任何问题，放到整个工程中就出现问题。
  
 
 
  
  
 
 
  通
  
   过查
  
  看内存，
  
   发现
  
  
   NSMutableArray
  
  中存
  
   储
  
  的
  
   为
  
  
   NSNumber
  
  指
  
   针
  
  ，array1和array2中指
  
   针
  
  地址一致，有可能是当
  
   [array1
 removeAllObjects];
  
  后，
  
   item
  
  有可能被自
  
   动销毁
  
  ，
  
   导
  
  致加入到array1中的
  
   值
  
  出
  
   现错误。修改为：
  
 
 
  NSNumber* topItem = [NSNumber numberWithInt: value];
 
 
  [array1 addObject:topItem];
 
 
  
   问题
  
  解决。
  
   这
  
  个
  
   问题
  
  的根本原因
  
   还
  
  是内存的
  
   问题
  
  ，objective-c的内存管理比
  
   较
  
  特殊，有很多
  
   隐藏
  
  的内存分配和自
  
   动
  
  回收，个人
  
   怀
  
  疑
  
   numberWithInt
  
  方法返回的
  
   NSNumber*
  
  
   对象为
  
  autorelease。
 


