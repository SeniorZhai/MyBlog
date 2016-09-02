title: UIControl
date: 2014-02-20 15:33:50
categories: iOS
tags: [iOS]
---
- UIControl
    + 作用：具有事件处理的控件的父类
    + 事件响应的3种形式：基于触摸、基于值、基于编辑
- 常用方法
```Objective-C
-(void)addTarget:(id)taget action:(SEL)action forControlEvent:(UIControlEvents)controlEvents //添加一个事件
-(void)removeTarget:(id)target action:(SEL)action forControlEvents:(UIControlEvents)controlEvents // 移除某一个事件
```
- 事件处理
```Objective-C
// 按下时触发
UIControlEventTouchDown
// 多击触发
UIControlEventTouchDownRepeat
// 控件内拖动触发
UIControlEventTouchDragInside
// 控件外拖动触发
UIControlEventTouchDrageOutside
// 当触摸从控件外拖动到控件内部时
UIControlEventTouchDragEnter
// 当触摸从控件内拖动到控件外部时
UIControlEventTouchDragExit
// 控件之内触摸抬起时
UIControlEventTouchupInside
// 控件之外触摸抬起时
UIControlEventTouchUpOutside
// 触摸事件取消
UIControlEventTouchCancel
// 当控件的值发生改变时
UIControlEventTouchValueChanged
// 文本控件中开始编辑
UIControlEventEditingDidBegin
// 文本控件中的文本被改变
UIControlEventEditingChanged
// 文本孔家中编辑结束
UIControlEventEditingDidEnd
// 文本孔家中通过按下回车键结束编辑时
UIControlEventEditingDidOnExit
// 所有触摸事件
UIControlEventAlltouchEvents
// 所有编辑事件
UIControlEventAllEditingEvents
// 所有事件
UIControlEventAllEvents
```

## 导航控制器
- 基本概念
    +`UINavigationController`用于构建分层应用程序的主要工具，管理着多个内容视图的换入（压入）和换出（弹出）。自身提供了视图切换的动画效果。
    + 它的父类是`UIViewController`，是所有视图控件器的基类。
    + 导航控制器以栈的形式来实现。
![](https://github.com/zt1991616/blog/raw/master/Image/14022004.png)
- 栈的基本概念和性质
    + 栈是一种先进后出（后进先出）的数据结构，导航控制器以栈的形式来管理视图控制器，任何视图控制器都可以放入栈中。
    + 向栈添加一个对象的操作称为入栈（push）
    + 第一个入栈的对象叫做基栈
    + 最后一个入栈的对象叫栈顶
    + 栈删除一个对象的操作叫做出栈（pop）
    + 当前现实点视图控制器，即为栈顶。选择"返回"时，这个视图控制器就出栈了。
- 基本样式
    + 蓝色部分：导航控制器的导航栏（NavigationBar）
    + 橙色部分：控制器包含的内容视图
    + 绿色部分：导航控制器的工具栏（UIToolBar，默认是隐藏的）
![](https://github.com/zt1991616/blog/raw/master/Image/14022005.png)
- 元素尺寸
![](https://github.com/zt1991616/blog/raw/master/Image/14022006.png)
- 代码示例
```Objective-c
// 控制器的初始化，为控制器添加导航控制器
RootViewController *rootVC = [[RootViewController alloc] init];
UINavigationController *navigation = [[UINavigationController alloc] initWithRootViewController:rootVC];
// 子控制器设置title，显示在导航栏上的标题
self.title = @"根控制器"；
// 控制器之间的导航
SecondViewController *secondVC = [[SecondViewController alloc] init];
[self.navigationController pushViewController:secondVC animated:YES];
[secondVC release];
// 隐藏（显示）导航栏、工具栏目
[self.navigationController setNavigationBarHidden:NO animated:YES];
[self.navigationController setToolbarHidden:NO animated:YES];
// 延时调用hidden方法
[self performSelector:@selector(hidden) withObject:nil afterDelay:0.3];
```
- Demo代码片段
```Objective-c
// AppDelegate.m
// didFinshLunchingWithOptions()
// ...
RootViewcController * rootViewController = [[RootViewcController alloc] init];
UINavigationController *navigation = [[UINavigationController alloc]initWithRootViewController:rootViewController];
self.window.rootViewController = navigation;
[rootViewController release];
[navigation release];
//RootController.m 
-(void)loagView
{
	UIView *baseView = [[UIView alloc] initWithFrame:[UIScreen mainScreen] applicationFrame];
	baseView.backgroundColor = [UIColor purpleColor];
	self.view = baseView;
	[baseView release];

	UIButton *button = [UIButton buttonWithType:UIButtonTypeRoundedRect];
	[button setTitle:@"Push" forState:UIControlStateNormal];
	[button setFrame:CGRectMake(90,100,140,35)];
	[button addTaget:self action:@selector(pushVC) forControlEvents:UIControlEventTouchUpInside];
	[self addSubview:button];
}
-(void)pushVC
{
	SencondViewController *secondVC = [[SencondViewController alloc] init];
	[self navigationController pushViewController:secondVC animated:YES];
	[secondVC release];
}
//SencondViewController.m
-(void)loagView
{
	UIView *baseView = [[UIView alloc] initWithFrame:[UIScreen mainScreen] applicationFrame];
	baseView.backgroundColor = [UIColor orangeColor];
	self.view = baseView;
	[baseView release];

	UIButton *button = [UIButton buttonWithType:UIButtonTypeRoundedRect];
	[button setTitle:@"HiidenOrShow" forState:UIControlStateNormal];
	[button setFrame:CGRectMake(90,150,140,35)];
	[button addTaget:self action:@selector(hiddenOrShow) forControlEvents:UIControlEventTouchUpInside];
	[self addSubview:button];

	UIButton *back = [UIButton buttonWithType:UIButtonTypeRoundedRect];
	[back setTitle:@"backRootVC" forState:UIControlStateNormal];
	[back setFrame:CGRectMake(90,200,140,35)];
	[back addTaget:self action:@selector(hiddenOrShow) forControlEvents:UIControlEventTouchUpInside];
	[self addSubview:back];
}
-(void)viewWillAppear:(BOOL)animated
{
	[super viewWillAppear:animated];
	[self.navigationController setToolBarHidden:YES animated:YES];
}
-(void)hiddenOrShow
{
	if(self.navigationController.toolbarHidden)
	{
		[self.navigationController setToolBarHidden:NO animated:YES];
		[self.navigationController setNavigationBarHidden:NO animated:YES];
	}else{
		[self.navigationController setToolBarHidden:YES animated:YES];
		[self.navigationController setNavigationBarHidden:NO animated:YES];
	}
}
-(void)backRootVC
{
	[self.navigationController popViewControllerAnimated:YES];
}
```
- 导航控制器常用属性
```Objective-C
// 获取到在栈中最顶层的视图控制器
@property(nonatomic,readonly,retain)UIViewController *topViewController;
// 获取到在斩重当前显示的视图控制器
@property(nonatomic,readonly,retain)UIViewController *visibleViewController;
// 在栈中当前视图控制器
@property(nonatomic,copy)NSAraay *viewControllers;
// 隐藏导航栏，默认不隐藏
@property(nonatomic,getter=isNavigationBarHidden)BOOL navigationBarHidden;
// 获取到导航栏
@property(nonatomic,readonly)UINavigationBar *navigatuonBar;
```
- 导航控制器常用方法
```Objective-C
// 初始化一个根视图控制器，在栈的最底层
-(id)initWithRootViewController:(UIViewController *)rootViewController;
// 压入一个新的视图控制器
-(void)pushViewController:(UIViewController *)viewController animated:(BOOL)animated;
// 弹出一个新的视图控制器
-(UIViewController *)popViewControllerAnimated:(BOOL)animated;
// 弹出到指定的视图控制器
-(NSArray *)popToViewController:(UIViewController *)viewController animated:(BOOL)animated;
// 回到跟视图控制器
-(NSArray *)popToRootViewControllerAnimated:(BOOL)animated;
```