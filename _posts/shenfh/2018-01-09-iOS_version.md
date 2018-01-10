---
layout: post
title: iOS 版本兼容
author:  shefh
catalog: true
date:  2018-01-08 07:17:17
tags:
    - iOS 
header-img: img/blog-0.jpg
---

## 直接获取系统版本
```
NSString *version = [UIDevice currentDevice].systemVersion;
if (version.doubleValue >= 9.0) {
    // 针对 9.0 以上的iOS系统进行处理
} else {
    // 针对 9.0 以下的iOS系统进行处理
}
```
## 通过Foundation框架版本号
通过 `NSFoundationVersionNumber` 判断API 的兼容性

### 定义
定义文件路径：`#import<Foundation/NSObjecRuntime.h>`


```
#define NSFoundationVersionNumber10_0	397.40
#define NSFoundationVersionNumber10_1	425.00
#define NSFoundationVersionNumber10_1_1	425.00
#define NSFoundationVersionNumber10_1_2	425.00
#define NSFoundationVersionNumber10_1_3	425.00
#define NSFoundationVersionNumber10_1_4	425.00
#define NSFoundationVersionNumber10_2	462.00
#define NSFoundationVersionNumber10_2_1	462.00
#define NSFoundationVersionNumber10_2_2	462.00
#define NSFoundationVersionNumber10_2_3	462.00
#define NSFoundationVersionNumber10_2_4	462.00
#define NSFoundationVersionNumber10_2_5	462.00
#define NSFoundationVersionNumber10_2_6	462.00
#define NSFoundationVersionNumber10_2_7	462.70
#define NSFoundationVersionNumber10_2_8	462.70
``` 

## 系统宏
### __IPHONE_OS_VERSION_MIN_REQUIRED 
当前支持的最小系统版本。值等于`Deployment Target`

```
#if __IPHONE_OS_VERSION_MIN_REQUIRED >= 80000
    //minimum deployment target is 8.0, so it’s safe to use iOS 8-only code
    当前SDK最小支持的设备系统，即8.0，所以在iOS 8.0设备上是安全的

#else
    //you can use iOS8 APIs, but the code will need to be backwards
    //compatible or it will crash when run on an iOS 7 device
    你仍然可以使用iOS 8的API，但是在iOS 7的设备上可能会crash.
#endif
```

### __IPHONE_OS_VERSION_MAX_ALLOWED 

当前支持的最大系统版本。值等于`Base SDK`。

```
#if __IPHONE_OS_VERSION_MAX_ALLOWED >= __IPHONE_11_0
    //you can use iOS 11 APIs here because the SDK supports them
    //but the code may still crash if run on an iOS 8 device
    可以使用最新的iOS 11的API，开始支持的新功能。但是仍然可能会在iOS 8的设备上crash。
#else
    //this code can’t use iOS 10 APIs as the SDK version doesn’t support them
    不能使用iOS 11的API，只能使用iOS 11之前的。
#endif
```

### 系统版本宏

系统版本宏可以判断当前系统版本是否是大于等于某个版本

```
#define __IPHONE_8_0      80000
#define __IPHONE_8_1      80100
#define __IPHONE_8_2      80200
#define __IPHONE_8_3      80300
#define __IPHONE_8_4      80400
#define __IPHONE_9_0      90000
#define __IPHONE_9_1      90100
#define __IPHONE_9_2      90200
#define __IPHONE_9_3      90300
#define __IPHONE_10_0    100000
#define __IPHONE_10_1    100100
#define __IPHONE_10_2    100200
#define __IPHONE_10_3    100300
#define __IPHONE_11_0    110000
#define __IPHONE_11_1    110100
#define __IPHONE_11_2    110200
```

```
#ifdef __IPHONE_8_0
// 系统版本大于 iOS8.0 执行
#endif

#ifdef __IPHONE_10_0
// 系统版本大于 iOS10.0 执行
#endif

```


## `@available` 运行时检查

```
    if (@available(iOS 11, *)) { // >= 11
        NSLog(@"iOS 11");
    } else if (@available(iOS 11, *)) { //>= 10
        NSLog(@"iOS 11");
    } else { // < 10
        NSLog(@" < iOS 10");
    }   
```

