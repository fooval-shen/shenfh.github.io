---
layout: post
title: UIStoryBoard 各个面板属性说明
author: shefh
category: blog
published: true
---

 使用UIStoryBoard很长一段时间了。自从用了它之后，就爱不释手了。UIStoryBoard 写UI笔记纯代码写UI效率要快非常多。而且很直观。如果是是写业务代码的话强烈建议还是使用UIStoryBoard 写布局代码，但是如果是写组件的话可以用VFL写布局代码。

 下面主要介绍下UIStoryBoard的面板属性


## file inspector


![fileInspector](/images/fileInspector.png) 

 * **Use Auto Layout**   是否使用autolayout布局
 * **Use Size Classes** 是否使用SizeClass，如果使用autolayout这个选项一般都要开起来
 * **Use as Launch Screen** 是否当做启动页。新建的工程默认是使用一个LaunchScreen.storyboard的文件做为启动页，如果想取消或者改用其他方式，首先需要到这边把Use as Launch Screen这个选项关掉才会生效。
 * **Global Tint** Stroyboard全局默认的tint color

## indentity inspector(标识区域)
### UIViewcontroller

 ![indentifySpector](/images/indentifySpector.png)

 * Cunstom Class 自定义UIView 或者Controller
  * **class** 自定的类名
  * **Module** 工程的模块名称。默认为当前工程名称。表示当前的class是隶属于那个模块的。模块名称在创建项目的时候就会生成，默认只有一个。如果想把一个工程编译成2个app，就需要重新建一个模块名。

  ![moduleinfo](/images/moduleinfo.png)

  * **Storyboard ID** Storyboard的一个标示，用于区分其他Storyboard。可以通过这个ID 加载一个ViewController。

{% highlight swift linenos %}
 if let vc = UIStoryboard(name: "main", bundle: nil).instantiateViewControllerWithIdentifier("viewID") as? ViewController {
       // do somethong
 }
{% endhighlight %}

 * **Restoration ID** 恢复标识，是系统进入后台或者应用被终止，app重新起来时的用于恢复时使用

 * **User Defined Runtime Attributes** 用户添加的在runtime的属性。比较简单的用法就是画圆角：

 ![runtimeAttribute](/images/runtimeAttribute.png)

 

## Attributes inspector （属性区域）
 这里可以设置View的大部分属性

 ![attributesSpector](/images/attributesSpector.png) 

 * **Simulated Metrics** 模拟度量。这个里面可以设置当前视图大小，是否横屏显示等。
 * **Is Initial View Controller** 设置StoryBoard的入口点。勾选上这个选项会有2个作用：
  * 如果把这个StoryBoard设置为 Main interface，那么app默认第一个加载这个界面不在需要手动是指rootViewController。
  * 可以通过以下方式加载一个布局

  {% highlight swift linenos %}
if let vc = UIStoryboard(name: "main", bundle: nil).instantiateInitialViewController() as? ViewController {
    //do something
}
  {% endhighlight %}


## continue...  
