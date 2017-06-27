---
title: Instruments分析加载图片占用的内存
date: 2016-12-11 23:25:32
tags:
---
加载图片的方法[UIImage imageName:@""],用得比较多,[UIImage imageWithContentsOfFile:]这个方法用得比较少,这是为啥呢?听我给你一一道来.

<!---more--->

### imageName: 
 1. 当图片控件 imageView 对象被销毁的时候, image 对象不会被销毁
 2. 这种加载图片的方式对内存的开销比较大: 12.1M
 3. 当有多个图片控件显示这个图片对象时,不会重复加载图片对象到内存中
 多个 UIImageView 引用同一个UIImage 对象,所以 UIImage 对象会常住内存中
 
 总结: 这种加载方法适合使用频率比较高得小图标,很多个界面都会用到的图片

###  imageWithContentsOfFile:
 1. 当图片控件 imageView 对象被销毁的时候, image 对象会被销毁
 2. 这种加载图片的方式对内存的开销比较小: 12.06 
 3. 有多个图片控件显示这个图片对象时,会冲出加载图片对象到内存中.这种方式是 UIImageView  <---> UIImage 是一一对应绑定的.UIImageView 消失了, UIImage 也就没了
 
 [总结]:适合那种使用频率低的大图的显示,比如启动页,APP 版本新特性呢场景使用这种方式
 



