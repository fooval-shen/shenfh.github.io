---
layout: post
title: UIWebView滑动事件被禁止后传递滑动事件的方法
author: shefh
category: blog
published: true
---

  之前项目上的需求需要把UIWebView的滑动事件给禁止掉。然而我们的h5页面做了一个这样的一个优化：h5在加载图片的时候是根据webview的滑动位置来加载还没有出现的在屏幕的图片的。
  
  解决办法是在外层scrollView滑动的时候传递下如下事件：
  
```
 id webBrowserView = self.webView.scrollView.subviews.firstObject;
    SEL sendEventSEL = NSSelectorFromString([NSString stringWithFormat:@"%@%@", @"sendScrollEvent", @"IfNecessaryWasUserScroll:"]);
    if ([webBrowserView respondsToSelector:sendEventSEL]) {
#pragma clang diagnostic push
#pragma clang diagnostic ignored "-Warc-performSelector-leaks"
        [webBrowserView performSelector:sendEventSEL withObject:@YES];
#pragma clang diagnostic pop
```

## [参考](  https://github.com/nst/iOS-Runtime-Headers/blob/f7f2c13158ff4ecd15b92eefb5c5d365b126db05/Frameworks/UIKit.framework/UIWebDocumentView.h#L696)

