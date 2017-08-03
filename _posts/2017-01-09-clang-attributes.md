---
layout: post
title: Objective-C Clang Arrtibutes常用属性
author: shefh
category: blog
published: true
---

* 目录  
{:toc #markdown-toc}

## `__attribute__((constructor))`

这个属性表示构造器,用于修饰函数，这个函数会在main函数之前运行。


```
__attribute__((constructor))
static inline void runBeforeMain(FHThreadBlock block) {
    if (block) {
        block();
    }
}
```

```
__attribute__((constructor(PRIORITY)))
PRIORITY 为优先级
```

## `__attribute__((objc_requires_super))`

表示方法需要调用super 方法。子类如果没用调用super方法会抛出警告。


```
@interface ViewController : UIViewController

- (void)prepareUI  __attribute__((objc_requires_super));
- (void)prepareData __attribute__((objc_requires_super));
@end
```

## `__attribute__((objc_subclassing_restricted))`

表示类不能被继承，实现了类似java final class 的效果。如果被继承，编译器会编译不过。


```
__attribute__((objc_subclassing_restricted))
@interface ViewController : UIViewController

- (void)prepareUI  __attribute__((objc_requires_super));
- (void)prepareData __attribute__((objc_requires_super));
@end
```

## `__attribute__((availability(...)))`

```
void f(void) __attribute__((availability(macosx,introduced=10.4,deprecated=10.6,obsoleted=10.7)));
```
这句话的意思是说函数f是在macosx 10.4版本引进的,然后在10.6版本被弃用了,并且将在10.7版本彻底废弃。

```
#define UNAVAILABLE_ATTRIBUTE __attribute__((unavailable))

#define NS_UNAVAILABLE UNAVAILABLE_ATTRIBUTE

- (instancetype)init NS_UNAVAILABLE
```


## __attribute__((unavailable))
```
__attribute__((unavailable("'c' must have the value of an unsigned char or EOF")))
``` 
方法或者对象不可用，会抛出警告.如果强行调用编译器会提示错误

## `__attribute__((always_inline))`

表示强制内联.内联函数在被调用的时候不会被编译成函数调用,而是直接扩展到调用函数体内.

```
#define force_inline __inline__ __attribute__((always_inline))
```

## __attribute__((deprecated))
方法弃用警告

```
__attribute__((deprecated))

__attribute__((deprecated(message)))
```

# 参考
[Attributes in Clang](http://releases.llvm.org/3.8.0/tools/clang/docs/AttributeReference.html)
[__attribute__](http://nshipster.com/__attribute__/)

