---
layout: post
title: NSURLSession 内存泄露的坑 
author: shefh
category: blog
published: true
---



今天用`NSURLSession`请求一个网络数据，然后被报了一个内存泄露的问题。具体代码如下：

{% highlight swift linenos %}
 NSURLSession *section = [NSURLSession sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                                                              delegate:self
                                                         delegateQueue:[[NSOperationQueue alloc] init]];
NSURLSessionDataTask *task = [section dataTaskWithURL:[NSURL URLWithString:path]
                                            completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {
                                                [self doResponseAction:data error:error];
                                            }];
[task resume];

{% endhighlight %}

然后对比下苹果的官方文档，意思是`NSURLSession`会对delegate强引用。所以必须在请求结束的时候invalidate当前section。

{% highlight ruby linenos %}
The session object keeps a strong reference to the delegate until your app 
exits or explicitly invalidates the session. If you do not invalidate the session, 
your app leaks memory until it exits.
{% endhighlight %}

所以解决办法有2个，一个是直接在`[task resume]`后面加上`[section finishTasksAndInvalidate];` ,还有一种方式是在`urlSession(_:didBecomeInvalidWithError:)`回调的时候添加`[section finishTasksAndInvalidate];`。

好的，很开心，泄露终于解决了。但是打开`Instruments`看下，尼玛啊，还是提示有泄露，然后不管你怎么释放sesion，就算你不设置代理，泄露还是一直存在，后面终于在`stackoverflow`上到了[答案](https://stackoverflow.com/questions/28223345/memory-leak-when-using-nsurlsession-downloadtaskwithurl),有兴趣的可以去看下。

我总结下，大意就是说每次new 一个`NSURLSession`的时候，苹果底层都会创建一个关于 SSL 的缓存。[ an SSL cache associated to your app, which takes 10 minutes to clear,](https://developer.apple.com/library/content/qa/qa1727/_index.html)。但是这个缓存基本需要10分钟后才能释放。所以，建议对于`NSURLSession`的创建使用单例模式，或者直接使用`[NSURLSession sharedSession]`.

然后自己写了一个demo，每次请求的时候重新穿件`NSURLSession`,结果如下
![demo](/images/sessionleakdemo.png)



