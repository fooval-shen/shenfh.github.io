---
layout: post
title: Core Graphics Tutorial Part 1-Getting Started(译文)
author:  shenfh
catalog: true
date:  2017-09-19 07:17:17
tags:
    - iOS 
    - 译文
header-img: img/blog-0.jpg
---

## 前言
这个是我第一尝试着去翻译一些国外的Blog，有很多地方估计翻译的不好。如果有发现没做好的地方欢迎大家留言。

> 原文地址：https://www.raywenderlich.com/162315/core-graphics-tutorial-part-1-getting-started 

想象一下你已经完成了你的`app`并且他运行的非常好，但是界面缺少一些风格。你可以用`Photoshop`画好几个自定义的控制图片然后你会希望`Apple`不会出4x的视网屏幕，或者你可以提前考虑在xcode中使用核心绘画来创建一张图片，它在任何的设备上被缩放都能保证清晰。

核心动态是`Apple`的矢量绘图框架。它是一个非常强大的`API`并且有很多东西需要去学习。但是不要害怕，这里的三部文章将会从简单开始带你进入核心绘画，并且最后，你可以在你的`app`上创造出令人惊艳的图片。

这是一个用现代的方法来教核心图形的全新的教程系列。这个教程系列还包含了很多非常棒的特性，比如`@IBDesignable`和`@IBInspectable`，它可以让核心图形的学习变得更加有趣和简单。

所以拿起你最喜欢的饮料，该开始了！

## 介绍Flo - 一次一杯
你将会创建一个完整的应用程序来记录你喝水的习惯。
具体来说，它可以很简单的追踪你喝了多少水。他们告诉我们每天喝8杯水是最健康的，但是我们很容易在喝了几杯后就会忘记之前喝了多少水。所以这个时候就用到了`Flo`，每次你喝完一杯清凉的水，点击计数器。你就可以看到之前七天的喝水曲线。

![1-CompletedApp](/img/Blog/CoreGraphics/1-CompletedApp.gif)

在这个系列教程的第一部分里面，你将会用`UIKit`的绘图方法创建三个控制器。

然后在第二部分中，你将更深入的了解核心图形的上下文，并且绘制一个曲线。

在第三部分里面，你将创建一个有图案的背景，让后给自己发放一个自制的核心图形奖章。

## 开始

你的第一个任务就是创建一个你自己的`Flo`应用程序。没有可以可以供下载的demo让你开始，因为如果你自己从头开始构建，你将会学到更多的东西。

创建一个新的工程`(File\New\Project…)`,选择模板`iOS\Application\Single View App`然后点击下一步。
填写项目选项的内容。把工程的名字命名为`Flo`,语言选择为`Swift`，然后点击下一步。

![Screen-Shot](/img/Blog/CoreGraphics/Screen-Shot-2017-06-25-at-10.43.38-PM.png)

在最后一个界面，不要选择`Create Git repository`，直接点击创建按钮。
现在你有了一个初始的工程，它包含了一个`Storyboard`和一个视图控制器。

## 在视图上自定义绘图

自定义绘图有三个步骤：

   1. 创建一个`UIView`的子类。
   2. 重载`draw(_:)`方法，然后在里面添加一些核心绘图的代码。
   3. 没有第三步了-就是这样了。
 
你将会创建一个加号按钮来试试这个，像这样的：

![1-AddButtonFinal](/img/Blog/CoreGraphics/1-AddButtonFinal.png)


创建一个新的文件(`File\New\File…`),选择`iOS\Source\Cocoa Touch Class`,然后点击下一步。在这个屏幕上，把新的类名命名为`PushButton`,把它设置为`UIButton`的一个子类，并且确保编程的语言为`Swift`。点击`Next`，然后点击`Create`。

`UIButton`是`UIView`的子类，所以所有`UIView`的方法，比如`draw(_:)`,都是可以在`UIButton`中被使用的。

在`Main.storyboard`,拖拽一个`UIButton`视图到视图控制器的视图里面，然后在` Document Outline`选择按钮。

在`Identity Inspector`区域，把类改成你自己的`PushButton`类.

![Screen-Shot](/img/Blog/CoreGraphics/Screen-Shot-2017-06-25-at-11.04.25-PM.png)


## 自动布局约束

现在，你将开始做自动布局约束：

![flo-button-1](/img/Blog/CoreGraphics/flo-button-1.gif)


  1. 在按钮选中的状态下，按住`control`键从按钮的中心稍微左拖动。(保持在按钮区域里面)，然后在弹出的菜单中选择`Width`。
  2. 同样的，在按钮选中的状态下，按住`control`从按钮的中心稍微往上拖动(保持在按钮区域里面),然后在弹出的菜单中选择`Height`。
  3. 按住`Control`从按钮的内部拖动到按钮区域之外，然后选择`Center Vertically in Safe Area.`。
  4. 最后按住`Control`按钮从按钮内部往上拖出到按钮的外部区域，然后选择`Center Horizontally in Safe Area`。

这将创建四个自动布局约束;现在你可以在`Size Inspector`看到它们：

![s2](/img/Blog/CoreGraphics/Screen-Shot-2017-08-28-at-20.20.29.png)

在` Align center Y`约束上点击`Edit`,把它的变量值改成100.这将会把按钮的垂直中心点从当前垂直中心点下移100个点。把`Width` 和`Height`约束值改成100。最后的约束会是这样子的：

![s3](/img/Blog/CoreGraphics/Screen-Shot-2017-08-28-at-20.12.39.png)

在` Attributes Inspector`区域，删除按钮的默认标题“Button”。

![1-RemoveTitle2](/img/Blog/CoreGraphics/1-RemoveTitle2.png)


这个时候如果你想的话你可以构建或者启动这个工程，但是这个时候你只能看到一个空白的屏幕。是时候去解决这个问题了。

## 绘制按钮

回想下你试着去创建的按钮是一个圆形的：
 
![1-AddButtonFinal](/img/Blog/CoreGraphics/1-AddButtonFinal.png)

用核心绘图绘制一个形状，你定义了一个`path`告诉核` Core Graphics`绘制的线条（就像+号的两条直线一样）或者填充的线(就像这个需要被填充的圆)。如果你对`Illustrator`或者`Photoshop`里面里面的矢量形状很熟悉，那你对路径就会很容易理解了。

关于路径有三级基本的知识需要知道：

  * `path`可以被描边和填充。
  * `stroke`在当前的描边颜色下描绘出路径。
  * `fill`将用当前的填充色填充成一个闭合路径。

一个创建绘画路径的简单办法就是通过使用一个便利的类叫做`UIBezierPath`。这个类可以让你通过一个友好的API非常简单就创建路径，无论你是想创建基于线条，曲线，矩形，还是一系列的连接点的路径。

尝试使用`UIBezierPath`来创建一个路劲，然后用绿色来填充它。为了这做我们需要打开`PushButton.swift`文件，添加如下代码：

```swift
override func draw(_ rect: CGRect) {
  let path = UIBezierPath(ovalIn: rect)
  UIColor.green.setFill()
  path.fill()
}
```

首先,你创建了一个椭圆形的`UIBezierPath`，他的大小是就是按钮矩形的大小。这个时候，它的大小会和`storyboard`上100*100的按钮大小一样，所以这个椭圆形事件会变成一个圆形。

路径他们自己并不会绘制任何东西。可以不需要任何可用的绘制上下文去定义一些路径。为了绘制出这个路径，你在当前上下文设置了填充颜色，让后填充了这个路径。

构建 然后启动应用程序，你将会见到一个绿色的圆。

![1-SimGreenButton2](/img/Blog/CoreGraphics/1-SimGreenButton2.png)


这个时候你就发现了自定义一个视图是多么简单的一件事情，就是通过创建一个`UIButton`子类，重载`draw(_:)`,然后把这个`UIButton`添加到`Storyboard`就好了。


## 核心图形的幕后场景

每一个`UIView`都有一个绘制上下文，所有视图的绘制都会在提交到设备的硬件之前渲染到到这个视图的上下文中。

iOS 在需要更新的视图的时候通过调用`draw(_:)`方法来更新上下文。这个场景发生在如下几个场景：

  * 视图在屏幕上被创建
  * 视图上面的其他视图被移动了
  * 视图的`hidden`属性被改变了
  * 应用程序直接调用了`setNeedsDisplay()`或者`setNeedsDisplayInRect()`方法。

  
>>注意
>在`draw(_:)`里面的任何绘制都会绘制到视图的上下文里面。请注意，如果你在`draw(_:)`方法之外绘制，就像你在本教程的最后一部分一样，你必须创建一个自己的绘制上下文。

在这一章节中你没有使用到`Core Graphics`是因为`UIKit`已经封装了很多`Core Graphics`的函数。例如`UIBezierPath`,它就是对`CGMutablePath`的封装。`CGMutablePath`其实是一个非常底层的`Core Graphics`的API。

>>注意：永远不要直接调用`draw(_:)`方法。如果你的视图没有被更新，那就在当前视图上调用`setNeedsDisplay() `更新。
>>`setNeedsDisplay()`方法不会调用`draw(_:)`方法，但是他会标记视图为需要更新，然后会在下一个屏幕刷新循环里调用`draw(_:)`触发视图重绘。尽管你在一个方法里面调用了`setNeedsDisplay() `5次，实际只会触发`draw(_:)`调用一次。


## @IBDesignable – 交互式绘图

创建绘制路径代码然后启动应用程序看它的效果到底是怎么样的，这就像是看着油漆变干一样令人兴奋，但是你还有其他的选择。` Live Rendering`通过调用视图的`draw(_:) `的方法可以让视图在`storyboard`上绘制更加的准确。而且`storyboard`会立即更新在`draw(_:)`产生的变化。你所需要的就只有一个简单的属性！

还是在`PushButton.swift`文件里面，只要在类的声明之前添加如下代码：

```swift
@IBDesignable
```


这就是所有开启及时渲染所需要要所用步骤。回到`Main.storyboard`文件中，注意了你的按钮这个时候被显示成一个绿色的圆形，就和你之前构建后启动起来的效果是一样。

现在设置好你的屏幕，这样你就可以把`storyboard`和代码并排放在一起了。
选中`PushButton.swift`后会显示源代码，让后在右上角，点击`Assistant Editor`-这个图标看起来像两个相互缠绕的圆环。`storyboard`会在右手边显示。如果没有，你就必须在面板顶部的面包屑路径中选择`storyboard`。

![1-Breadcrumbs](/img/Blog/CoreGraphics/1-Breadcrumbs.png)


关闭`Storyboard`左边的文档大纲，腾出一些空间出来。通过拖动文档大纲区域的边缘点击`Storyboard`底部的按钮也腾出一些空间。
![1-DocumentOutline](/img/Blog/CoreGraphics/1-DocumentOutline.png)

当所有的步骤你都做了之后，你的屏幕应该是像这样子的：
![s4](/img/Blog/CoreGraphics/Screen-Shot-2017-08-28-at-20.45.08.png)

在`PushButton`的`draw(_:)`方法里面做一些变更

```swift
把
UIColor.green.setFill()
改成
UIColor.blue.setFill()
```

然后你会立即(或者过一会)看到在`Stoaryboard`上面的变化。相当酷！
![s5](/img/Blog/CoreGraphics/Screen-Shot-2017-08-28-at-20.45.01.png)

现在你将会去创建加号的线条。

## 绘制到上下文

核心绘画使用了画家的的模式。当你绘制到上下文的时候，就像画一幅画一样。你铺设了一条路径然后把它填充满，然后你在这个路径上铺设另外一个路径把它填充满。你不能改变已经铺设好路径的像素点，但是你可以在它们上面重新画画。

下面的图片是`Apple`的文档中描述的绘制到上下文是怎么工作的。就像你在画布上作画一样，你所画的顺序是至关重要的。

![1-PaintersModel](/img/Blog/CoreGraphics/1-PaintersModel.gif)

你的加号是绘制到蓝色圆形上面的，所以第一步你用代码绘制了蓝色的圆形然后再绘制加号。

你可以画两个矩形来表示加号，但是画一条路径更容易，然后用设计好的厚度来描边。
添加下面的结构体和变量到`PushButton`里面：

```swift
private struct Constants {
  static let plusLineWidth: CGFloat = 3.0
  static let plusButtonScale: CGFloat = 0.6
  static let halfPointShift: CGFloat = 0.5
}
  
private var halfWidth: CGFloat {
  return bounds.width / 2
}
  
private var halfHeight: CGFloat {
  return bounds.height / 2
}
```


现在你可以添加下面的代码到`draw(_:) `方法后面去画加号的横线:

```swift
//set up the width and height variables
//for the horizontal stroke
let plusWidth: CGFloat = min(bounds.width, bounds.height) * Constants.plusButtonScale
let halfPlusWidth = plusWidth / 2

//create the path
let plusPath = UIBezierPath()

//set the path's line width to the height of the stroke
plusPath.lineWidth = Constants.plusLineWidth

//move the initial point of the path
//to the start of the horizontal stroke
plusPath.move(to: CGPoint(
  x: halfWidth - halfPlusWidth,
  y: halfHeight))

//add a point to the path at the end of the stroke
plusPath.addLine(to: CGPoint(
  x: halfWidth + halfPlusWidth,
  y: halfHeight))

//set the stroke color
UIColor.white.setStroke()

//draw the stroke
plusPath.stroke()

```


在这个阶段里，你创建了一个`UIBezierPath`，设定了一个起始坐标点让后绘制到终点。然后你用白色填充了这个路径。这个时候你会在`Storyboard`看到的效果。
在你的`storyboard`将会在蓝色的圆形中间看到一条横线。

![Dash](/img/Blog/CoreGraphics/Dash.png)


>注意:
>记住，路径是由许多个点简单的组合在一起的。这里有一个很简单的方法来理解这个概念:当你创建路径时，想象一下你手里拿着一支笔。在纸上画上两个点，然后把笔放在起始点上，然后画一条线连接到下一个点。这就是你使用` move(to:)`和`addLine(to:)`本质上所做的事情。会有一个调淡蓝色的线围绕着它。

现在在你的` iPad 2`或者` iPhone 6 Plus`模拟器上运行应用程序，然后你会发现这个横线并没有它应该的那么清晰。
![](https://koenig-media.raywenderlich.com/uploads/2014/12/1-PixelledLine.png)

## 点与像素

在第一代iphone的时候，点和像素占据了相同的空间，大小相同，本质上是一样的。当视网膜`iPhone`出现后，屏幕上的同一个点突然出现了4倍的像素点。
同样的，`iPhone 6 Plus`再次增加了相同点的像素数量。


>注意：
>下面的内容是概念上的——实际的硬件像素可能有所不同。例如，在渲染了3x图片之后，`iPhone 6 Plus`会降低像素的采样来屏幕上显示完整的图像。想要了解更多关于`iPhone 6 Plus`的缩减像素采样，请查看[这篇文章](https://www.paintcodeapp.com/news/iphone-6-screens-demystified)。


下面有一张12×12像素的网格，其中的点以灰色和白色显示。第一个(iPad 2)是点和像素是相映射的。第二个(iPhone 6)是一个2x的视网膜屏幕，其中一个点上有4个像素，第三个(iPhone 6 Plus)是一个3x视网膜屏幕，在一个点上有9个像素。

![1-Pixels](/img/Blog/CoreGraphics/1-Pixels.png)


你刚才画的线是3个点。线在路径的的中心开始绘制，所以在路径的中心线的两边都画出了1.5个点。
这张图片展示了在每个设备上画的三个点宽的线。你可以看到在`iPad 2`和`iPhone 6 Plus`上这条线的边缘被画在了半个像素点上——当然，这是无法做到的。所以,iOS用两种颜色的平均值抗锯齿被填充一般的像素 ，这条线看起来很就模糊。

![1-PixelLineDemonstrated](/img/Blog/CoreGraphics/1-PixelLineDemonstrated.png)


实际上，`iPhone 6 Plus`的像素太多了，你可能不会注意到它的模糊性，尽管你应该在设备上检查一下自己的应用。但如果你正在开发`iPad 2`或`iPad mini`这样的非视网膜屏的应用程序时，你应该尽可能的避免抗锯齿。

如果你奇数大小的直线，你需要通过加上或者减去0.5个点去放置它们以防止抗锯齿。如果你看一下上面的图表，你会发现`iPad 2`上的一个半点会把这条线向上移动半像素，在iPhone 6上，会上升1整个像素，在iPhone 6 Plus上，会增加1.5个像素。
在`draw(_:)`方法里，把`move(to:)`和`addLine(to:)`里面的代码替换成这样:

```swift
//move the initial point of the path
//to the start of the horizontal stroke
plusPath.move(to: CGPoint(
  x: halfWidth - halfPlusWidth + Constants.halfPointShift,
  y: halfHeight + Constants.halfPointShift))
    
//add a point to the path at the end of the stroke
plusPath.addLine(to: CGPoint(
  x: halfWidth + halfPlusWidth + Constants.halfPointShift,
  y: halfHeight + Constants.halfPointShift))
```


iOS将会在这三种设备上呈现出明显的线条，因为你现在已经将这条路径移动了0.5个点。
>iOS will now render the lines sharply on all three devices because you’re now shifting the path by half a point.

>注意：
>对于像素完美的线条，您可以绘制并填充一个`UIBezierPath(rect:)`而不是一条直线，并使用视图的`contentScaleFactor`来计算矩形的宽度和高度。不像从路径中心向外画的笔画，只填充路径内的区域。

在前面的两行代码之后加上加号的竖直线，在`draw(_:)`之前设置绘制的颜色。我敢打赌，你可以自己弄清楚怎么做，因为你已经画出了水平的直线了。

```swift
plusPath.move(to: CGPoint(
  x: halfWidth + Constants.halfPointShift,
  y: halfHeight - halfPlusWidth + Constants.halfPointShift))
      
plusPath.addLine(to: CGPoint(
  x: halfWidth + Constants.halfPointShift,
  y: halfHeight + halfPlusWidth + Constants.halfPointShift))
```

现在你可以在`Storyboard`中及时的看到加号的显示。这就完成了加号按钮的绘制。


![1-FinishedPlus](/img/Blog/CoreGraphics/1-FinishedPlus.png)

##  @IBInspectable – 自定义`Storyboard`属性

你知道在疯狂的时候你会点击非常多次按钮，只是为了确保你确实点击按钮成功了?这个时候你需要给用户一个修改这个多次点击的事件——你需要一个减号按钮。

减号和加号其实是一样的，只是减号没有竖直的横线并且它的颜色会不一样。你将会给这个减号按钮使用相同的`PushButton`类，然后在添加到`storyboard`的时候指定它是什么类型的按钮和它的颜色。

`@IBInspectable`是一个属性，你可以将它添加到`Interface Builder`可以识别的属性里面。这就是说你可以在`storyboard`上直接配置按钮的颜色，而不是在代码中配置。
在`PushButton`类的顶部添加下面两个属性：

```swift
@IBInspectable var fillColor: UIColor = UIColor.green
@IBInspectable var isAddButton: Bool = true
```

把`draw(_:)`上面的填充颜色从

```swift
UIColor.blue.setFill()
```

更改成:

```swift
fillColor.setFill()	

```

在`storyboard`的视图上按钮会变成绿色。
在` draw(_:)`方法里面有`if`语句把竖直线的绘制代码包裹起来.

```swift
//Vertical Line
if isAddButton {
  //vertical line code move(to:) and addLine(to:)
}
//existing code
//set the stroke color
UIColor.white.setStroke()
plusPath.stroke()
```
这就使得只有`isAddButton`属性被设置为`TRUE`后才会显示竖直线-这样按钮就可以是加号按钮也可以是减号按钮了。
完整的`PushButton`类的代码是这样的：

```swift
import UIKit

@IBDesignable
class PushButton: UIButton {
  
  private struct Constants {
    static let plusLineWidth: CGFloat = 3.0
    static let plusButtonScale: CGFloat = 0.6
    static let halfPointShift: CGFloat = 0.5
  }
  
  private var halfWidth: CGFloat {
    return bounds.width / 2
  }
  
  private var halfHeight: CGFloat {
    return bounds.height / 2
  }
  
  @IBInspectable var fillColor: UIColor = UIColor.green
  @IBInspectable var isAddButton: Bool = true
  
  override func draw(_ rect: CGRect) {
    let path = UIBezierPath(ovalIn: rect)
    fillColor.setFill()
    path.fill()
    
    //set up the width and height variables
    //for the horizontal stroke
    let plusWidth: CGFloat = min(bounds.width, bounds.height) * Constants.plusButtonScale
    let halfPlusWidth = plusWidth / 2
    
    //create the path
    let plusPath = UIBezierPath()
    
    //set the path's line width to the height of the stroke
    plusPath.lineWidth = Constants.plusLineWidth
    
    //move the initial point of the path
    //to the start of the horizontal stroke
    plusPath.move(to: CGPoint(
            x: halfWidth - halfPlusWidth + Constants.halfPointShift,
            y: halfHeight + Constants.halfPointShift))
        
    //add a point to the path at the end of the stroke
    plusPath.addLine(to: CGPoint(
            x: halfWidth + halfPlusWidth + Constants.halfPointShift,
            y: halfHeight + Constants.halfPointShift))

    if isAddButton {
      //move the initial point of the path
      //to the start of the horizontal stroke
      plusPath.move(to: CGPoint(
        x: halfWidth - halfPlusWidth + Constants.halfPointShift,
        y: halfHeight + Constants.halfPointShift))
      
      //add a point to the path at the end of the stroke
      plusPath.addLine(to: CGPoint(
        x: halfWidth + halfPlusWidth + Constants.halfPointShift,
        y: halfHeight + Constants.halfPointShift))
    }
    
    //set the stroke color
    UIColor.white.setStroke()
    plusPath.stroke()
  }
}
```

在`storyboard`上，选中`PushButton`类型的视图。你用`@IBInspectable`声明的两个属性出现在`Attributes Inspector`的顶部：

![1-InspectableFillColor](/img/Blog/CoreGraphics/1-InspectableFillColor.png)


把`Fill Color`改成`RGB(87, 218, 213)`,然后把`Is Add Button`的值设置为`off`。到`Fill Color\Other…\Color Sliders`修改颜色，在每个颜色输入框中输入颜色的值，然后它像这样：

![color-picker-275x500](/img/Blog/CoreGraphics/color-picker-275x500.png)

这些变化会在`Storyboard`中立即发生。

![1-InspectableMinusButton](/img/Blog/CoreGraphics/1-InspectableMinusButton.png)

相当的酷是吧？现在把` Is Add Button`的值改回到`on`，把它变回成加号按钮。

## 第二个按钮

在`Storyboard`上添加一个新的按钮，然后选中它。和之前的按钮一样把它的类改成`PushButton`类:

![s6](/img/Blog/CoreGraphics/Screen-Shot-2017-06-25-at-11.04.25-PM.png)

绿色的加号按钮将会被绘制到旧的加号按钮下面。
在`Attributes Inspector`区域，把`Fill Color`改成`RGB(238, 77, 77)`,把`Is Add Button`的值设置为`off`。删除按钮的默认标题`Button`。

![MinusButtonColor](/img/Blog/CoreGraphics/1-MinusButtonColor.png)

和之前一样给新的视图添加自动布局约束:

   1. 在按钮选中的状态下，按住`control`键从按钮的中心稍微左拖动。(保持在按钮区域里面)，然后在弹出的菜单中选择`Width`。
  2. 同样的，在按钮选中的状态下，按住`control`从按钮的中心稍微往上拖动(保持在按钮区域里面),然后在弹出的菜单中选择`Height`。
  3. 按住`Control`从按钮的内部拖动到按钮区域之外，然后选择`Center Vertically in Safe Area.`。
  4. 按住`Control`按钮从按钮内部往上拖出到按钮的外部区域，然后选择`Vertical Spacing`。
  
约束添加后，对照下面图修在`Size Inspector`修改约束常量的值:

![s7](/img/Blog/CoreGraphics/Screen-Shot-2017-08-28-at-20.56.09.png)


构建后运行应用程序。你现在已经有一个可重复使用可自定义的的视图，你可以把它添加任何的app上面。它在任何设备上都非常清晰非常锋利。这个是在`iPhone 4S`上面额效果：

![SimPushButtons](/img/Blog/CoreGraphics/1-SimPushButtons.png)

## `UIBezierPath`弧线

下一个自定义的视图你将会创建成这样：

![CompletedCounterView](/img/Blog/CoreGraphics/1-CompletedCounterView.png)

这看起来像一个填满的形状，但是弧线实际上只是一个被填充得肥胖的路径。轮廓线是另一条由两条弧线组成的描边。

创建一个新的文件,` File\New\File…,`选择`Cocoa Touch Class`，然后把类名命名为`CounterView`。把它设置为`UIView`的子类，确保编程语言为`Swift`。点击下一步，最后点击`Create`。
替换文件里面代码：

```swift
import UIKit

@IBDesignable class CounterView: UIView {
  
  private struct Constants {
    static let numberOfGlasses = 8
    static let lineWidth: CGFloat = 5.0
    static let arcWidth: CGFloat = 76
    
    static var halfOfLineWidth: CGFloat {
      return lineWidth / 2
    }
  }
  
  @IBInspectable var counter: Int = 5
  @IBInspectable var outlineColor: UIColor = UIColor.blue
  @IBInspectable var counterColor: UIColor = UIColor.orange
  
  override func draw(_ rect: CGRect) {
    
  }
}
```


在这里你创建了包含一些常量的结构体。这些常量在绘制的时候回被用到，其中比较特殊的常量`numberOfGlasses`是每天需要喝的水的杯数。当达到这个数据的时候，计数器会指向最大值的位置。

同时你还创建了一些可以在`storyboard`上做更新的属性。变量`counter`用于记录已经喝的水的杯数，它是具有`@IBDesignable`属性，可以在`storyboard`改变它的值。特别的，它可以用来测试这个计数器视图。

移动到`Main.storyboard `,在加号`PushButton`按钮上面添加一个`UIView`视图。和之前一样给这个新的视图添加自动布局约束:

  1. 在按钮选中的状态下，按住`control`键从按钮的中心稍微左拖动。(保持在按钮区域里面)，然后在弹出的菜单中选择`Width`。
  2. 同样的，在按钮选中的状态下，按住`control`从按钮的中心稍微往上拖动(保持在按钮区域里面),然后在弹出的菜单中选择`Height`。
  3. 按住`Control`从按钮的内部拖动到按钮区域之外，然后选择`Center Vertically in Safe Area.`。
  4. 按住`Control`按钮从按钮内部往上拖出到按钮的外部区域，然后选择`Vertical Spacing`。

在`Size Inspector`区域修改约束常量的值：

![s8](/img/Blog/CoreGraphics/Screen-Shot-2017-08-28-at-21.00.28.png)


在`Identity Inspector`区域，把`UIView`改成`CounterView`。在`draw(_:)`代码里面的所有绘图都会显示在视图上(但是你还没有添加任何东西).
>In the Identity Inspector, change the class of the UIView to CounterView. Any drawing that you code in draw(_:) will now show up in the view (but you’ve not added any yet!).



## 临时的数学课程

我们打断了这篇教程，简单地说了一下，希望不要害怕回顾高中的数学知识。就像`Douglas Adams`说的——别慌！
在上下文中绘制是基于这个单位圆的。这个单位圆是一个半径为1.0的圆。

![FloUnitCircle](/img/Blog/CoreGraphics/1-FloUnitCircle.png)


红色的箭头显示了弧形的开始位置和结束位置，以顺时针方向画。你从`3π / 4`弧度的位置开始画，这个点等于` 135º`,顺时针到`π / 4 `的弧度也就是`45º`圆角的位置。

在程序里面经常使用弧度而不是角度，用弧度来思考是很有用的这样你就不用每次计算圆圈的时候就不用装换角度了。接下来你需要指出弧形的长度，也就是弧度将会发挥作用的时候。
在单位圆上弧形的长度和测量出来的弧度是一样的。比如看上面的图表，弧形从`0º`到`90º`的长度是`π/2`。要计算弧形的实际长度，取单位圆弧长，并将其乘以实际半径。
要计算上面红色箭头的长度，你只需要计算它所占用的弧度值:

```ruby
 2π – end of arrow (3π/4) + point of arrow (π/4) = 3π/2
```

用角度来计算应该是：

```ruby
 360º – 135º + 45º = 270º
```

![s9](/img/Blog/CoreGraphics/Screen-Shot-2017-08-28-at-21.07.40.png)
 

## 回去画圆弧

在`CounterView.swift`文件里面添加这些代码到`draw(_:) `来画一个圆弧:

```swift
// 1
let center = CGPoint(x: bounds.width / 2, y: bounds.height / 2)

// 2
let radius: CGFloat = max(bounds.width, bounds.height)

// 3
let startAngle: CGFloat = 3 * .pi / 4
let endAngle: CGFloat = .pi / 4

// 4
let path = UIBezierPath(arcCenter: center,
                           radius: radius/2 - Constants.arcWidth/2,
                       startAngle: startAngle,
                         endAngle: endAngle,
                        clockwise: true)

// 5
path.lineWidth = Constants.arcWidth
counterColor.setStroke()
path.stroke()
```

下面来解释下每个部分的作用:

   1. 定义圆弧旋转的中心点。
   2.  根据视图的最大尺寸计算圆弧的半径。
   3.  定义圆弧的开始和结束的角度。
   4. 根据之前定义的中心点，半径和角度创建一个路径。
   5. 在最后填充路径之前设置线条的宽度和颜色


想象一下用一个圆规来画这个——你把圆规固定的点放在中间，把圆规的支架张开到你需要的半径，装上一根厚厚的铅笔，然后旋转它来画出你的弧形。
在这代码里面，`center`是圆规的固定点，`radius`是圆规打开的宽度(减去笔的一半的宽度),让后圆弧的宽度就是铅笔的宽度。

>注意：
>当你画圆弧的时候，这些是你必须知道的，但是如果你想更加深入的画圆弧，那么`Ray`的[Core Graphics Tutorial on Arcs and Paths](http://www.raywenderlich.com/33193/core-graphics-tutorial-arcs-and-paths)的这篇文章或许可以帮助你

在`storyboard`和你的应用程序启动后，这个效果会是这样的：

![SimArcStroke](/img/Blog/CoreGraphics/1-SimArcStroke.png)

## 圆弧的轮廓

当用户表示他们喜欢喝一杯水时,基数器上的沦落显示了向着八杯水目标的进度。
这个轮廓线包含了两个弧线，一个在外面另一个在里面，让后有两根线连接着它们。

在`CounterView.swift`文件里面，把这些代码添加到`draw(_:)`后面:

```swift
//Draw the outline

//1 - first calculate the difference between the two angles
//ensuring it is positive
let angleDifference: CGFloat = 2 * .pi - startAngle + endAngle
//then calculate the arc for each single glass
let arcLengthPerGlass = angleDifference / CGFloat(Constants.numberOfGlasses)
//then multiply out by the actual glasses drunk
let outlineEndAngle = arcLengthPerGlass * CGFloat(counter) + startAngle

//2 - draw the outer arc
let outlinePath = UIBezierPath(arcCenter: center,
                                  radius: bounds.width/2 - Constants.halfOfLineWidth,
                              startAngle: startAngle,
                                endAngle: outlineEndAngle,
                               clockwise: true)

//3 - draw the inner arc
outlinePath.addArc(withCenter: center,
                       radius: bounds.width/2 - Constants.arcWidth + Constants.halfOfLineWidth,
                   startAngle: outlineEndAngle,
                     endAngle: startAngle,
                    clockwise: false)
    
//4 - close the path
outlinePath.close()
    
outlineColor.setStroke()
outlinePath.lineWidth = Constants.lineWidth
outlinePath.stroke()
```

这边做了几件事情:

  1. `outlineEndAngle`是弧线结束的角度，它的值是根据`counter`计算出来的。
  2. `outlinePath`是外弧线。由于圆弧不是一个单位圆，所以需要给一个半径给`UIBezierPath() `计算圆弧的实际长度。
  3. 在第一个圆弧上增加一个内弧。它们有相同的角度，但以相反的方向画(顺时针方向被设置为`false`)。同样，这也会自动地在内外弧线之间画一条线。
  4. 关闭路径会自动在弧线的结束点和起始点之间画一条线。

在`ounterView.swift`文件把`counter`属性的值设置为`5`，在`storyboard`上面的`CounterView`应该会是这个样子:

![ArcOutline](/img/Blog/CoreGraphics/1-ArcOutline.png)

打开`Main.storyboard`文件，选择`CounterView`,在`Attributes Inspector`修改`Counter`属性的值来检测下你的绘制代码。你会发现它是完全交互式的。试着把计数器的值设置成大于8或者小于0的数值。你后面会去修复它的。

把`Counter Color`的值修改成`RGB(87, 218, 213)`,把`Outline Color`的值修改成`RGB(34, 110, 100)`。

![CounterView](/img/Blog/CoreGraphics/1-CounterView.png)

## 让所有的工作起来

祝贺下。你现在已经有了所有的控制单元。你所需要做的就是把他们了解起来，让加号按钮添加计数器的值，减号按钮减小计数器的值。

在`Main.storyboard`文件，拖一个`UILabel`到`Counter View`的中心并且确保它是`Counter View`的一个子视图。在文档区域它会是想这样子。

在`label`垂直和水平中心点添加约束。最后，这个`label`应该有这样的约束：
 
 ![](https://koenig-media.raywenderlich.com/uploads/2017/06/Screen-Shot-2017-06-27-at-1.02.34-PM.png)

在`Attributes Inspector`区域，把`Alignment`属性值改成`center`，字体改成`36`,然后把默认的标题改成`8`.

![LabelAttributes](/img/Blog/CoreGraphics/1-LabelAttributes.png)

到`ViewController.swift`文件，在类的顶部添加下面这些属性:

 ```swift
 //Counter outlets
@IBOutlet weak var counterView: CounterView!
@IBOutlet weak var counterLabel: UILabel!
 ```
 
还是在`ViewController.swift`，添加下面方法到类的结尾：

```swift
@IBAction func pushButtonPressed(_ button: PushButton) {
  if button.isAddButton {
    counterView.counter += 1
  } else {
    if counterView.counter > 0 {
      counterView.counter -= 1
    }
  }
  counterLabel.text = String(counterView.counter)
}
```

在这里你根据按钮的`isAddButton`属性来增加或者减少`counter`的值。确保这个计数器的值不会被减到小于0-没有人会喝负数的水。同时更新了在`label`的计数器的值。

同时把这段代码添加到`viewDidLoad()`,确保`counterLabel`的初始值被更新。

```swift
counterLabel.text = String(counterView.counter)  
```

在`Main.storyboard`文件，连接`CounterView` 和`UILabel`的出口。给两个`PushButton`按钮连接`Touch Up Inside `事件方法。

![ConnectingOutlets2](/img/Blog/CoreGraphics/1-ConnectingOutlets2.gif)

启动应用程序看下计数`label`是否会被按钮更新。它们应该是可以被更新的。

但是等下，为什么计数视图没有被更新？

回想下这篇文章的开始，当视图上面的子视图被移除，或者视图的`hidden`属性被改变，或者视图被新创建到屏幕，或者你的app在视图上调用了`setNeedsDisplay()` 或者`setNeedsDisplayInRect()`方法的时候你是怎么调用`draw(_:)`方法的。

不管怎么样计数值被更新后计数视图也同样需要被更新，否自用户会认为你的app崩溃了。

到`CounterView.swift`文件，把`counter`属性的声明改成这样子：

```swift
@IBInspectable var counter: Int = 5 {
  didSet {
    if counter <=  Constants.numberOfGlasses {
      //the view needs to be refreshed
      setNeedsDisplay()
    }
  }
}
```

这段代码使得视图只有当计数器小于或等于用户的目标杯数时才会刷新，因为轮廓只会上升到8。

再次启动你的应用程序。现在一切都可以正常的运行了。

![Part1Finished](/img/Blog/CoreGraphics/1-Part1Finished.png)

## 结束

[完成的demo工程](https://koenig-media.raywenderlich.com/uploads/2017/06/Flo-Part1-Xcode9b2.zip)






