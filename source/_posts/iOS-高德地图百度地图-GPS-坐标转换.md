---
title: iOS 高德地图百度地图实现后台定位
date: 2018-01-04 16:02:56
tags:
    - iOS
---

> 坐标体系

* WGS84: 大地坐标系,广泛使用的 GPS 定位系统采用的坐标系.
* GCJ02: 火星坐标系,由中国测绘局对 WGS84进行加密后得到的坐标系.
* BD09: 百度坐标,针对火星坐标再次加密所得BD09II 位百度经纬度坐标, BD09mc 为百度墨卡托米制坐标

<!---more--->

> 地图 SDK 采用哪种坐标体系

|      |百度地图 SKD  |高德地图 SKD  |GPS 设备(苹果,安卓手机)|
| ---  | --- | --- | --- |
| WGS84|     |     |   √ |
| GCJ02|     |  √[查看](http://lbs.amap.com/faq/top/coordinate/3/?wd=%E5%9D%90%E6%A0%87&cateId=&page=&detail=true)  |     |
| BD09 |   √[查看](http://lbsyun.baidu.com/index.php?title=iossdk/guide/coordtrans)|     |     |

> ios实现后台定位

1. 需求: 要求定时上传用户的经纬度,无论App 在前台还是后台.
2. 准备工作
    * info.plist 文件中添加必要字段
        - ~~NSLocationUsageDescription~~已经用不到了
        - NSLocationWhenInUseUsageDescription(简单的前台定位)
        - NSLocationAlwaysUsageDescription(后台定位,要适配 iOS10之前的版本这个也要添加上)
        - NSLocationAlwaysAndWhenInUseUsageDescription(后台定位iOS10及以后)
    * 开启后台定位开关
        - TARGETS -->Capbilities -->Background Modes打开
        - 选中Location updates 
    
3. 编写代码[参考官方](https://developer.apple.com/documentation/corelocation/choosing_the_authorization_level_for_location_services/requesting_always_authorization?language=objc)
    * 创建定位管理者
    
        ```
        - (CLLocationManager *)locationManager
        {
            if (_locationManager == nil)
            {
            _locationManager = [[CLLocationManager alloc]init];
            _locationManager.delegate = self;
            _locationManager.distanceFilter = kCLDistanceFilterNone;
            _locationManager.allowsBackgroundLocationUpdates = YES;
            _locationManager.desiredAccuracy = kCLLocationAccuracyBestForNavigation;
            }
            return _locationManager;
        }   
        ```
        
    * 请求授权,并开始定位
    
        ```
        - (void)startUpdateLocation
        {
            //请求后台定位授权
            [self.locationManager requestAlwaysAuthorization];
            dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(3 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
                [self.locationManager startUpdatingLocation];
            });
        }
        ```
    
> 踩坑

1. 授权后台定位弹窗
    ![Simulator Screen Shot - iPhone X - 2018-01-05 at 15.51.31](http://7xtc4k.com1.z0.glb.clouddn.com/Simulator Screen Shot - iPhone X - 2018-01-05 at 15.51.31.png)

    * 解决办法
        - requestAlwaysAuthorization之后过几秒钟后执行 startUpdatingLocation方法.
    
    * 踩到这个坑的原因.
        - 不知道 info.plist 文件中的几个 key 的含义
        - 不知道这个方法的含义requestAlwaysAuthorization 请求后台定位授权
        - 不知道这个方法的含义requestWhenInUseAuthorization 请求前台定位授权
        - 请求后台定位授权后,立即执行startUpdatingLocation,仅仅弹出前台授权的弹窗(这个坑找了好久好久).
    
2. 蓝条出现的情况
    * 在执行后台定位,但是后台定位权限就会出现蓝条
    * 查看 APP 当前定位权限的方法
        - 设置 -->隐私 -->定位服务 -->查看自己的 APP 的授权
    * 执行的代码大于当前的授权的时就会出现蓝条
3. 崩溃发生在 startUpdateLocation
    * TARGETS -->Capbilities -->Background Modes关闭再打开一次
    
> 坐标体系转换

1. 网上已经有现成的轮子了[坐标转换](https://github.com/JackZhouCn/JZLocationConverter)
2. 坐标转换:
    * WGS84 <-->数学运算 <--> GCJ02 <-->数学运算 <-->BD09
3. 放出我的 [demo](https://github.com/maker997/LocationDemo)

    
    
    


    






