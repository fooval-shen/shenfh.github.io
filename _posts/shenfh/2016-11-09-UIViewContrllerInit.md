---
layout: post
title: UIViewController初始化过程
author: shefh
catalog:  true
tags:
    - iOS
---

UIViewController的初始化有三种方式，分别是**代码初始化**，从**xib初始化**和从**Storyboard初始化**。

其中**代码初始化**和**xib初始化**基本相同。

## 初始化方法
初始化方法分别涉及到如下方法：
* <h6>init</h6>
* <h6>initWithCoder</h6>
* <h6>initWithNibName</h6>
* <h6>awakeFromNib</h6>
* <h6>loadView</h6>
* <h6>ViewDidLoad</h6>

## Storyboard加载视图
 首先来看一个Demo：
{% highlight swift %}
//Storyboard加载
@implementation ViewController
 - (instancetype)init {
     NSLog(@"ViewController --  init");
    self = [super init];
    if(self) {
        
    }
   
    return self;
}
- (instancetype)initWithCoder:(NSCoder *)aDecoder {
    NSLog(@"ViewController --  initWithCoder");
    self = [super initWithCoder:aDecoder];
    if (self ) {
        
    }
    return self;
}

- (instancetype)initWithNibName:(NSString *)nibNameOrNil bundle:(NSBundle *)nibBundleOrNil {
    NSLog(@"ViewController --  initWithNibName");
    self = [super initWithNibName:nibNameOrNil bundle:nibBundleOrNil];
    if (self) {
        
    }
    return self;
}
- (void)awakeFromNib {
    NSLog(@"ViewController --  awakeFromNib");
    [super awakeFromNib];
}
- (void)loadView {
     NSLog(@"ViewController -- loadView");
    [super loadView];
//    self.view = [[UIView alloc]init];
   
}
- (void)viewDidLoad {
    NSLog(@"ViewController --  viewDidLoad");
    [super viewDidLoad];
}
@end

// 执行结果为：
 ViewController --  initWithCoder
 ViewController --  awakeFromNib
 ViewController -- loadView
 ViewController --  viewDidLoad
{% endhighlight %}
 
<div>

 从执行结果来看，初始化的时候没有执行<code>initWithNibName</code> 方法,为什么呢？看下Apple的文档： 
{% highlight swift  %}
This is the designated initializer for this class. When using a storyboard to
define your view controller and its associated views, you never initialize your
view controller class directly. Instead, view controllers are instantiated by
the storyboard either automatically when a segue is triggered or
programmatically when your app calls the instantiateViewControllerWithIdentifier: 
method of a storyboard object. When instantiating a view controller from a storyboard,
iOS initializes the new view controller by calling its initWithCoder: method instead
of this method and sets the nibName property to a nib file stored inside the storyboard.

The nib file you specify is not loaded right away. It is loaded the first time
the view controller's view is accessed. If you want to perform additional
initialization after the nib file is loaded, override the viewDidLoad method
and perform your tasks there.If you specify nil for the nibName parameter
and you do not override the
loadView method, the view controller searches for a nib file as described in the
nibName property.
{% endhighlight %}
</div>

* 从Apple的文档我们知道，当我们从Storyboard加载view的时候，他不会调用<code>initWithNibName</code>，而是调用<code>initWithCoder</code>这个初始化方法。所以Storyboard加载UIViewController的过程应该是这样的：
 	调用<code>initWithCoder</code>初始化，然后调用<code>awakeFromNib</code>说明ViewController加载结束(注意：这个时候ViewController的各个IBOutlet属性都没有加载完)。然后在调用<code>loadView</code>,最后调用<code>viewDidLoad</code>方法。
 	
* <code>loadView</code>是一个比较特殊的方法。当访问ViewController 的view时，如果view 非nil，那么会直接返回这个view；如果view是 nil 的，那么 View Controller 会调用它的 loadView 方法来加载并把对象赋值给view。如果我们重载了<code>loadView</code> 并且没有调用父类的<code>loadView</code>导致view属性为nil，系统就会一直调用<code>loadView</code>方法，知道程序崩溃。

## 代码初始化

{% highlight swift %}
TestViewController *vc = [[TestViewController alloc]init];
[self addChildViewController:vc];

@implementation TestViewController
- (instancetype)init {
    NSLog(@"TestViewController --  init");
    self = [super init];
    if(self) {
        
    }    
    return self;
}
- (instancetype)initWithCoder:(NSCoder *)aDecoder {
     NSLog(@"TestViewController --  initWithCoder");
    self = [super initWithCoder:aDecoder];
    if (self ) {
        
    }   
    return self;
}
- (instancetype)initWithNibName:(NSString *)nibNameOrNil bundle:(NSBundle *)nibBundleOrNil {
     NSLog(@"TestViewController --  initWithNibName");
    self = [super initWithNibName:nibNameOrNil bundle:nibBundleOrNil];
    if (self) {
        
    }   
    return self;
}
- (void)loadView {
    NSLog(@"TestViewController -- loadView");
    [super loadView];    
}
- (void)awakeFromNib {
     NSLog(@"TestViewController --  awakeFromNib");
    [super awakeFromNib];   
}
- (void)viewDidLoad {
    NSLog(@"TestViewController --  viewDidLoad");
    [super viewDidLoad];
}
@end
// 执行结果
 TestViewController --  init
 TestViewController --  initWithNibName
 TestViewController -- loadView
 TestViewController --  viewDidLoad
{% endhighlight %}

* 从执行接过来来看，初始化时先执行<code>init</code>,再执行<code>initWithNibName</code>.代码初始化为什么会执行<code>initWithNibName</code>方法呢？
原来代码初始化的时候，<code>init</code>会自动调用<code>initWithNibName</code>方法，如果<code>nibNameOrNil</code>参数不为空的时候，会去寻找制定xib加载<code>UIViewController</code>.如果<code>nibNameOrNil</code>参数为空的时候会去寻找和这个类名相同的xib加载,如果再找不到的话，执行默认初始化。
* 这个时候不会调用<code>awakeFromNib</code>这个方法，因为这个方法只有从nib加载的时候才会调用。



