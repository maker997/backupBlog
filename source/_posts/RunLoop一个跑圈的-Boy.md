---
title: RunLoop一个跑圈的 Boy
date: 2016-12-18 13:15:25
tags:
---
>线程,RunLoop有什么关系呢?

一个线程自带一个RunLoop boy.一个应用程序至少有一个主线程,它的 RunLoop boy 系统给创建好了,并时刻待命准备干活.程序自己开启的普通线程,也带有跑圈的 boy. 不过这个跑圈的boy 干完活就睡觉去了.如果这个 boy 睡觉去,他的主人线程也会被系统回收.

<!---more--->

>认识下这个跑圈的 boy 吧

RunLoop 这个 boy 有很多模式(mode),简单的可以理解为这个这种模式下只干某种类型的工作.
`NSDefaultRunLoopMode` 默认模式
`UITrackingRunLoopMode`scrollView 滚动的时候进入该模式
`NSRunLoopCommonModes`正常模式.
可以供程序员修改 boy 的模式的就这三种,其中前两种模式是互斥的,都属于第三种模式.

>RunLoop 这个 boy 干什么活啊?

这个 boy 干三种工作.
1. NSTimer 定时器的工作
2. source 各种事件
3. Obersever 观察者

NSTimer 这个最好理解,给RunLoop这个 boy 添加一个定时器,这个 boy 就会每隔一段时间去处理 NSTimer

source 简单的理解为事件.又可以详细分为source0和source1.source0 是系统的事件,比如其他线程传来的消息或者事件.source1为用户事件.比如用户点击屏幕上的按钮触发的事件.从函数调用栈的来看,RunLoop 总是先处理系统的事件,然后处理 source1.

Obersever 这个观察者不是普通的观察者,它是用来观察RunLoop这个 boy的状态的.RunLoop这个 boy 转态发生变化时就会通着观察者.这个观察者对于开发有什么用处呢?来来来...哥告诉你,当你想拦截点击事件方法之前做点事情,就可以通过这个观察者来动手

>RunLoop 有几种状态

![progerss](http://7xtc4k.com1.z0.glb.clouddn.com/progerss.png)

>RunLoop 几种应用场景

1. 定时器的使用
2. 创建长驻线程监听网络状态,或者沙盒变化
3. 监听拦截点击事件





