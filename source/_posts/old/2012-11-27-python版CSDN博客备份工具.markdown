---
layout: post
title: "python版CSDN博客备份工具"
date: 2012-11-27 10:44:00 
comments: true
categories: [Python]
tags: [Python]
description: "python版CSDN博客备份工具"
keywords: Python
---


 前一阵子看了gzshun的一篇文章《自己动手编写CSDN博客备份工具-blogspider》，恰巧当时正在学习python，遂萌发了用python写一个类似的备份工具。由于当时项目太忙，导致这个想法一直被搁置到项目尾声，正好这几天bug不多，《python核心编程》基础部分也都看完了，于是就有了这个程序。
 
  这里只记录下程序实现中遇到的问题，全部程序会上传到下载中，希望对python学习者能有所帮助。
  
   
    第一个问题是http库的使用的问题，最初我选择了urllib库，因为其接口简单明确，使用起来非常方便，但是就是无法访问CSDN博客，估计是csdn blog对类似spider进行了过滤。没办法只能退而求其次使用urllib2库，指定request的headers就可以顺利访问csdn blog了。
    
         i_headers = {"User-Agent": "Mozilla/5.0 (Windows; U; Windows NT 6.0; zh-CN; rv:1.9.1) Gecko/20090624 Firefox/3.5"}
    urllib2.Request('http://blog.csdn.net/goof', headers=headers)
    page = urllib2.urlopen(req)
    page.read() #我的blog的主页内容
     
      第二个问题是对字符串中特殊字符过滤问题，由于名字可能会有一些特殊字符，会导致无法创建文件或文件夹，所以必须要对字符串进行过滤。必须是对unicode的字符串的过滤。
      
           not_letters_or_digits = u'!"#%\'()*,-./:;<=>;?@[\]^_`{|}~ \n\r'
    translate_table = dict((ord(char), u'') for char in not_letters_or_digits)
    mystring.translate(translate_table)
       
        第三个问题是html parser，最初使用的是python自带的HTMLParser库，结果遇到当html body过大无法处理的问题，最后选择了第三方的Beautifulsoup库，一个强大而又方便使用的库。
        
         
          最后的遗憾是在对正则表达式的使用上，自己还是非常的生疏，一遍查找资料一遍测试，最终虽然勉强完成了功能，但是还是对程序的机构和效率方面不慎满意。不过这也是个好事，可以继续升级维护这个工具，让它慢慢完美！
          
           
            
             
              完整程序的下载地址：http://download.csdn.net/detail/goof/4805444
             
             
              希望大家多批评指导。ps：python2.7.3 beautifulsoup-3.2.1
             
             
              
               
                
                
               
              
             
            
           
          
         
        
       
      
     
    
   
  
 


