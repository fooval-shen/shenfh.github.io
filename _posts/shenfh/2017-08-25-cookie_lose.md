---
layout: post
title:   iOS  App退出后cookie被清除
author:  shefh
catalog: false
date:       2017-08-25 11:17:17
tags:
    - iOS
header-img: img/blog-0.jpg
---

今天在用`NSHTTPCookie`保存cookie的时候发现每次app退出的时候，cookie被 系统清除了，保存cookie的代码如下：

```Objective-c
- (void)addCookieForDomain:(NSString *)domain value:(NSString *)value {
    NSMutableDictionary *cookieDic = [[NSMutableDictionary alloc] init];
    [cookieDic setObject:kCookeiAppendName forKey:NSHTTPCookieName];
    [cookieDic setObject:value forKey:NSHTTPCookieValue];
    [cookieDic setObject:[NSDate dateWithTimeIntervalSinceNow:3*60 * 60] forKey:NSHTTPCookieExpires];
    [cookieDic setObject:domain forKey:NSHTTPCookieDomain];
    [cookieDic setObject:@"/" forKey:NSHTTPCookiePath];
    [cookieDic setObject:@(0) forKey:NSHTTPCookieVersion];
    [cookieDic setObject:@(TRUE) forKey:NSHTTPCookieDiscard];

    NSHTTPCookie *newCookiew = [[NSHTTPCookie alloc] initWithProperties:cookieDic];
    [[NSHTTPCookieStorage sharedHTTPCookieStorage] setCookie:newCookiew];
}

```

然后调试了下终于发现了原因，是因为创建的cookie调试的时候发现创建的cookie `expiresDate`总是为 null。奇怪了有设置`NSHTTPCookieExpires`的失效期，为什么会为null？

```Ruby
<NSHTTPCookie version:0 name:"access_token" value:"12345qwer--08aaa" expiresDate:(null) created:2017-08-25 08:40:40 +0000 sessionOnly:TRUE domain:"a.test-cookie.com" partition:"none" path:"/" isSecure:FALSE>
```

几经调试，发现如果把`  [cookieDic setObject:@(1) forKey:NSHTTPCookieDiscard];`这行代码注释掉，`expiresDate`就不会了。为什么呢？好吧，开始深入分析。。。

查看了下苹果文档对`NSHTTPCookieDiscard`定义：
>An NSString object stating whether the cookie should be discarded at the end of the session.
String value must be either "TRUE" or "FALSE". This cookie attribute is optional. The default is "FALSE", unless this cookie is version 1 or greater and a value for NSHTTPCookieMaximumAge is not specified, in which case it is assumed to be "TRUE"

一看到这个描述我很开心，那不是把`NSHTTPCookieDiscard`值改为`FALSE`就好了。实验证明还是错的，改为`FALSE`cookie还是被清除了。
然后认真看了下`NSHTTPCookie`各个属性发现了苹果对`sessionOnly`描述

>@property(readonly, getter=isSessionOnly) BOOL sessionOnly;
Description	
A boolean value that indicates whether the receiver should be discarded at the end of the session (regardless of expiration date).
YES if the receiver should be discarded at the end of the session (regardless of expiration date), otherwise NO.

在创建cookie的时候，只要带上了`NSHTTPCookieDiscard`属性`sessionOnly`就会为`TRUE`，所以必须去掉`NSHTTPCookieDiscard`.

