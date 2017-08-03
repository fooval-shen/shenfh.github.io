---
layout: post
title: 关于 "Content Compression Resistance" 和 "Content Hugging"
author: shefh
category: blog
published: true
---

* 目录  
{:toc #markdown-toc}

在使用autolayout的时候，我们经常会看到“Content Compression Resistance”和“Content Hugging”这个配置选项，他们是做什么用的呢？

![autolayout image](/images/autolayout.png) 


## Intrinsic Size
  要理解"Content Compression Resistance" 和 "Content Hugging"首先要知道"Intrinsic Size"即"固有尺寸".它是一个根据自身内容大小而决定的尺寸。比如UILabel 和 UIButon 我们有时候只需要设置top 和left 的约束就能自动撑开或者换行。

  **引用自苹果官方Auto Layout指南里面对内部内容尺寸的描述：**


{% highlight  linenos %}
Intrinsic Content Size

Leaf-level views such as buttons typically know more about what size they should be than does the code that is positioning them. This is communicated through the intrinsic content size, which tells the layout system that a view contains some content that it doesn’t natively understand, and indicates how large that content is, intrinsically.

For elements such as text labels, you should typically set the element to be its intrinsic size (select Editor > Size To Fit Content). This means that the element will grow and shrink appropriately with different content for different languages. 

{% endhighlight %}


## Content Compression Resistance Priority(内容压缩阻力优先级)
 * 优先级越高，则越晚轮到被压缩。如果不想视图被先压缩的话，把这个值调高就好了。

{% highlight swift linenos %}
 private lazy var nameLabel: UILabel = {
    let label = UILabel()
    label.setContentHuggingPriority(UILayoutPriorityDefaultHigh + 1, forAxis: .Horizontal)
    label.translatesAutoresizingMaskIntoConstraints = false    
    return label;
 }()
{% endhighlight %}

## Content Hugging Priority(内容紧靠优先级)

 * 该优先级越高，这越晚轮到被拉伸


