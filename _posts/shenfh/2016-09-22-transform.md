---
layout: post
title: 仿射变换CGAffineTransform详解
author: shefh
catalog:  false
tags:
    - iOS
---

* 目录  
{:toc #markdown-toc}

 今天在用CGAffineTransform对一个UIView进行旋转，碰到了一些问题。特此研究了下CGAffineTransform，做下记录。

## 开始

CGAffineTransform是实现了二维空间上UIView的平移，旋转，缩放等效果.它和CATransform3D（3D立体旋转矩阵）相似.结构体如下：

{% highlight swift %}
 public struct CGAffineTransform {
    public var a: CGFloat
    public var b: CGFloat
    public var c: CGFloat
    public var d: CGFloat
    public var tx: CGFloat
    public var ty: CGFloat
    public init()
    public init(a: CGFloat, b: CGFloat, c: CGFloat, d: CGFloat, tx: CGFloat, ty: CGFloat)
 }
{% endhighlight %}


### 仿射矩阵(3*3矩阵)的计算方式
    
```ruby
  			[a  b  0]
 [x` y` 1] = [x y 1] *	[c  d  0]      
    			[tx ty 1]
```
 结果为：

{% highlight swift %}
x' = a*x + c*y + tx
y' = b*x + d*y + ty
{% endhighlight %}

### 说明
 * **a** 表示水方向(x)的缩放 
 * **b** 表示垂直方向(y)进行拉伸（UIView的高度跟着改变）
 * **c** 表示水平方向(x)进行拉伸 (UIView的宽度是跟着改变)
 * **d** 表示垂直方向(y)的缩放 
 * **tx** 表示水平方向(x)的偏移
 * **ty** 表示垂直方向(y的偏移
 * **缩放和拉伸是不会改变中心点的**

## CGAffineTransform方法

 * **CGAffineTransformIdentity** 是一个单位矩阵，一般用来初始化一个反射对象

{% highlight swift %}
 /* The identity transform: [ 1 0 0 1 0 0 ]. */

 @available(iOS 2.0, *)
 public let CGAffineTransformIdentity: CGAffineTransform
{% endhighlight %}


* **CGAffineTransformMakeTranslation** 从矩阵的结构可以看出，这个函数的作用是实现一个视图的平移。

{% highlight swift %}
 /* Return a transform which translates by `(tx, ty)':
     t' = [ 1 0 0 1 tx ty ] */

 @available(iOS 2.0, *)
 public func CGAffineTransformMakeTranslation(tx: CGFloat, _ ty: CGFloat) -> CGAffineTransform
{% endhighlight %}


* **CGAffineTransformMakeScale** 实现视图水平或者竖直方向的缩放。

{% highlight swift %}
 /* Return a transform which scales by `(sx, sy)':
     t' = [ sx 0 0 sy 0 0 ] */

 @available(iOS 2.0, *)
 public func CGAffineTransformMakeScale(sx: CGFloat, _ sy: CGFloat) -> CGAffineTransform
{% endhighlight %}


* **CGAffineTransformMakeRotation** 根据旋转度旋转视图。

{% highlight swift %}
 /* Return a transform which rotates by `angle' radians:
     t' = [ cos(angle) sin(angle) -sin(angle) cos(angle) 0 0 ] */

 @available(iOS 2.0, *)
 public func CGAffineTransformMakeRotation(angle: CGFloat) -> CGAffineTransform
{% endhighlight %}

旋转度可以通过以下常量来计算：

{% highlight swift %}
 public var M_LOG2E: Double { get } /* log2(e)        */
 public var M_LOG10E: Double { get } /* log10(e)       */
 public var M_LN2: Double { get } /* loge(2)        */
 public var M_LN10: Double { get } /* loge(10)       */
 public var M_PI: Double { get } /* pi             */
 public var M_PI_2: Double { get } /* pi/2           */
 public var M_PI_4: Double { get } /* pi/4           */
 public var M_1_PI: Double { get } /* 1/pi           */
 public var M_2_PI: Double { get } /* 2/pi           */
 public var M_2_SQRTPI: Double { get } /* 2/sqrt(pi)     */
 public var M_SQRT2: Double { get } /* sqrt(2)        */
 public var M_SQRT1_2: Double { get } /* 1/sqrt(2)      */

 CGAffineTransformMakeRotation(CGFloat(M_PI))	//旋转180度
 CGAffineTransformMakeRotation(CGFloat(M_PI_2))	//旋转90度
{% endhighlight %}


* **CGAffineTransformInvert**  反向的仿射矩阵.比如（x，y）通过矩阵t得到了（x',y'）那么通过这个函数生成的t'作用与（x',y'）就能得到原始的(x,y)。可以用于还原反射。

{% highlight swift %}
 /* Invert `t' and return the result. If `t' has zero determinant, then `t'
   is returned unchanged. */

 @available(iOS 2.0, *)
 public func CGAffineTransformInvert(t: CGAffineTransform) -> CGAffineTransform
{% endhighlight %}

* **CGAffineTransformConcat** 反射组合。可以把旋转和缩放这两个放射动作通过GAffineTransformConcat组合在一起。

{% highlight swift %}
 /* Concatenate `t2' to `t1' and return the result:
     t' = t1 * t2 */

 @available(iOS 2.0, *)
 public func CGAffineTransformConcat(t1: CGAffineTransform, _ t2: CGAffineTransform) -> CGAffineTransform
{% endhighlight %}





