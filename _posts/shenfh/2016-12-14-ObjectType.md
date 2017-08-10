---
layout: post
title: Objective-C 自定义泛型
author: shefh
catalog:  false
tags:
    - iOS
---

最近在看在看RAC的源码，才知道Objective-C现在也支持自定义泛型，特此记录下自定义泛型的使用。

{% highlight swift %}
@interface RACSignal<__covariant ValueType> : RACStream

@end
{% endhighlight %}


自定义泛型只能在 @interface 上定义（类声明、类扩展、Category）,这个类型在 @interface 和 @end 区间的作用域有效，可以认为是一个模板。模板名称可以自己定义。如下：

{% highlight swift %}

NS_ASSUME_NONNULL_BEGIN
@interface UserModel<__covariant ObjectType> : NSObject

@property(nonatomic,strong,nullable) ObjectType object;
- (void)pushObject:(ObjectType)object;
- (__kindof ObjectType)value;
@end

NS_ASSUME_NONNULL_END
{% endhighlight %}

默认情况下，系统会把ObjectType当做id类型来处理。当然我们也可给泛型限制类型。
{% highlight swift %}

NS_ASSUME_NONNULL_BEGIN
@interface UserModel<__covariant ObjectType:NSNumber *> : NSObject

@property(nonatomic,strong,nullable) ObjectType object;
- (void)pushObject:(ObjectType)object;

@end

NS_ASSUME_NONNULL_END
{% endhighlight %}

## __covariant 和 __contravariant
 * __covariant - 协变性，子类型可以强转到父类型。 如果有两个数据 UserModel<NSArray *> a 和 UserModel<NSMutableArray*> b,那么b复制给a是不会有警告的，但是a赋值给b的话就会出现警告。
 * __contravariant - 逆变性，父类型可以强转到子类型。同理，a 赋值给b是不会有警告的。

## __kindof 表示是否是该类型的数据
{% highlight swift %}
@property (nonatomic, readonly, copy) NSArray<__kindof UIView *> *subviews;

UIButton *button = view.subviews.lastObject;
{% endhighlight %}
有了__kindof button 赋值就不会有警告了。


