---
layout: post
title: Objective-C nil Nil Null和NSNull的区别
author: shefh
category: blog
published: true
---

* 目录  
{:toc #markdown-toc}

本质nil，Nil，NULL和NSNull的值一样的,都表示空数据。
nil,Nil,NULL 指针指向的内容是一样的，都是0x00。NSNull是一个对象，表示空对象。


| 标志 | 值 | 含义 |
| --- | --- | --- |
| nil | (id)0 | Objective-C对象的字面零值 |
| NULL | (void *)0 | C指针的字面零值 |
| Nil | (Class)0 | Objective-C类的字面零值 |
| NSNull | [NSNull null] | 用来表示零值的单独的对象 |

# nil
nil 主要是给Objective-c 对象使用

```
id objectValue = nil;
TestObject *Object = nil;
```
### 说明：
刚被创建NSObject对象里面的所有对象指针都是指向nil。nil比较奇怪，虽然它是空数据，但是他还是可以有消息发送给它而且不会崩溃。


``` 
id nilObject = nil;
int intvalue = [nilObject integerValue];
double doubleValue = [nilObject doubleValue];
id first = [nilObject firstItem];

/**
    intvalue = 0
    doubleValue = 0
    first = nil
*/
```

# NULL
NULL主要用于赋值c指针或者是基本数据类型

```
int *pointerInt = NULL；
char *pointerToChar = NULL;
int intValue = NULL;
```

# Nil
Nil主要用于Objective-c 类

```
Class  class = Nil；
```

# Null

NSNull表示空对象,一般用于表示集合中值为空的对象。

```
NSMutableDictionary *dicValue = [[NSMutableDictionary alloc]init];    
[dicValue setObject:[NSNull null] forKey:@"111"];
[dicValue setObject:@(2) forKey:[NSNull null] ];
```



