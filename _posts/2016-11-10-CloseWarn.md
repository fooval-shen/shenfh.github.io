---
layout: post
title: iOS 关闭警告配置
author: shefh
category: blog
published: true
---

Object-c 经常会出现一些方法弃用或者方法找不到的警告，去掉这些警告可以使用 <code>#pragma clang diagnostic</code>宏定义

{% highlight swift linenos %}
#pragma clang diagnostic push
#pragma clang diagnostic ignored "-相关命令"
// 警告代码块
// ...
#pragma clang diagnostic pop
{% endhighlight %}

## 方法弃用告警
{% highlight swift linenos %}
#pragma clang diagnostic push  
#pragma clang diagnostic ignored "-Wdeprecated-declarations"      
// 警告代码块  
#pragma clang diagnostic pop
{% endhighlight %}

## 不兼容指针类型警告

{% highlight swift linenos %}
#pragma clang diagnostic push   
#pragma clang diagnostic ignored "-Wincompatible-pointer-types"  
// 警告代码块
#pragma clang diagnostic pop
{% endhighlight %}

## 找不到方法警告
{% highlight swift linenos %}
#pragma clang diagnostic push
#pragma clang diagnostic ignored "-Wundeclared-selector"
// 警告代码块
#pragma clang diagnostic pop
{% endhighlight %}

## 未使用变量警告

{% highlight swift linenos %}
#pragma clang diagnostic push   
#pragma clang diagnostic ignored "-Wunused-variable"  
// 警告代码块  
#pragma clang diagnostic pop
{% endhighlight %}


## 循环引用警告

{% highlight swift linenos %}
#pragma clang diagnostic push  
#pragma clang diagnostic ignored "-Warc-retain-cycles" 
// 警告代码块  
#pragma clang diagnostic pop
{% endhighlight %}

## 对象弱引用警告

{% highlight swift linenos %}
#pragma clang diagnostic push
#pragma clang diagnostic ignored "-Wreceiver-is-weak"
// 警告代码块  
#pragma clang diagnostic pop
{% endhighlight %}

## GNU编译警告
{% highlight swift linenos %}
#pragma clang diagnostic push
#pragma clang diagnostic ignored "-Wgnu"
 // 警告代码块 
#pragma clang diagnostic pop
{% endhighlight %}

## nullable 和 nonull警告
{% highlight swift linenos %}
NS_ASSUME_NONNULL_BEGIN
@interface UserModel<ObjectType> : NSObject

@property(nonatomic,strong,nullable) ObjectType object;
- (void)pushObject:(ObjectType)object;

@end

NS_ASSUME_NONNULL_END
{% endhighlight %}

## 参考资料
[警告命令](http://fuckingclangwarnings.com/)

