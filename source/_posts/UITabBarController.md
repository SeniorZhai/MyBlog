title: UITabBarController
date: 2014-02-24 12:41:38
categories: iOS
tags: [iOS]
---
- UITabBarController的基本概念
	+ UITabBarController和UINavigationController一样是用来管理视图控制器的。
	+ UINavigationController是用来管理视图之间的导航，UITabBarController是管理固定的几个视图控制器，子控制器是并列的。可以任意切换显示。
- UITabBarController基本样式
![](https://github.com/zt1991616/blog/raw/master/Image/14022401.png)
- UITabBarController初始化
```Objective-C

UIViewController *viewCtrl1 = [[UIViewController alloc] init];
viewCtrl.title = @"主页";

UIViewController *viewCtrl2 = [[UIViewController alloc] init];
viewCtr2.title = @"消息";

UIViewController *viewCtrl3 = [[UIViewController alloc] init];
viewCtr3.title = @"搜索";

UIViewController *viewCtrl4 = [[UIViewController alloc] init];
viewCtr4.title = @"设置";

NSArray *viewControllers = [NSArray arrayWitjObjects:viewCtr1,viewCtrl2,viewCtrl3,viewCtrl3,viewCtrl4,nil];
UITabBarController *mainViewController = [[UITabBarController alloc] init];
mainViewController.viewControllers = viewControllers;
[self.window setRootViewController:mainViewController];
```
- 示例代码
```Objective-C
/* ... */
/*
 * 1.创建若干个子视图控制器，它们是并列关系
 * 2.创建一个数组，将已创建的子视图控制器，添加到数组中
 * 3.创建UITabBarContrller实例
 * 4.tabBarController.viewContrllers = viewContollers;
 * 5.添加到window的rootViewController
 */
UIViewController *vc1 = [[UIViewController alloc]init];
vc1.title = @"首页"；
vc1.view.backgroundColor = [UIColor redColor];

UIViewController *vc2 = [[UIViewController alloc]init];
vc2.title = @"新闻"；
vc2.view.backgroundColor = [UIColor blueColor];

UIViewController *vc3 = [[UIViewController alloc]init];
vc3.title = @"历史"；
vc3.view.backgroundColor = [UIColor yellowColor];

UIViewController *vc4 = [[UIViewController alloc]init];
vc4.title = @"搜索"；
vc4.view.backgroundColor = [UIColor purpColor];

UIViewController *vc5 = [[UIViewController alloc]init];
vc5.title = @"设置"；
vc5.view.backgroundColor = [UIColor orangeColor];

NSArray *viewControllers = @[vc1,vc2,vc3,vc4,vc5];
UITabBarController tabBarController = [[UITabBarController alloc] init];
[UITabBarController setViewContronllers:viewControllers animated:YES];
self.window.rootViewController = tabBarController; 
```

- UITabBarController结构图
	+ Tab控制器用数组管理视图，视图间是平级的。
![](https://github.com/zt1991616/blog/raw/master/Image/14022402.png)

- TabBarController类图分析
	+ 一个分栏视图控制器控制着若干视图控制器，由一个数组管理
	+ 每个分栏控制器只有一个UITabBar视图，用于显示UITabItem实例
	+ UITabBarItem由当前的视图控制器管理
![](https://github.com/zt1991616/blog/raw/master/Image/14022403.png)

- UITabBarController系统样式
	+TabBar只能显示5个Tab Item，超过5个则会自动生成个More标签显示剩余的Tab，这些Tab可以通过编辑显示在UITabBar上。如果将视图添加到导航控制器中，默认出现编辑按钮，可以自由移动item实例。
![](https://github.com/zt1991616/blog/raw/master/Image/14022404.png)

## 实例代码
- 创建系统自带的TabBarController
```Objective-C
TabBarViewController *tabBarController = [[UITabBarViewController alloc]init];
FirstViewController *firstItem = [[FirstViewController alloc] init];
UITabBarItem *firstItem = [[UITabBarItem alloc]initWithTabBarSystemItem:UITabBarSystemItemFavorites tag:1];
firstViewController.tabBarItem = firstItem;
// ......
NSArray *viewControllers = @[firstItem,secondItem,thirdItem];
[tabBarController setViewControllers:viewControllers animated:YES];
self.window.rootViewController = tabBarController;
```

##自定义UITabBarItem
- 图片大小30*30px(视网膜屏60*60)
- 图片需要使用淡灰色或者半透明效果，选择系统自动填充蓝色
```Objective-C
FirstViewController *firstVC = [[FirstViewController alloc]init];
UITabBarItem *firstItem = [[UITabBarItem alloc] initWithTitle:@"主页" image:[UIImage imageNamed:@"image.ong"]tag:1];
firstVC.tabBarItem = firstItem;
```
- UITabBarController代理方法
```Objective-C
-(BOOL)tabBarController:(UITabBarController *)tabBarController shouldSelectViewController:(UIViewController *)viewController
{
	// 视图将被切换时调用，viewController为将被显示的控制器
}
- (void)tabBarController:(UITabBarController *)tabBarController didSelectViewController:(UIViewController *)viewController
{
// 视图已经被切换后调用，viewController为已经显示的控制器
}
```
##基层分栏控制器和导航控制器
- 在Tab Bar控制器中某一个Tab中使用Navigation控制器。
- 实现代码
```Objective-C
FirstViewController *firstVC = [[FirstViewController alloc] init];
UINavigationController * fNav = [[UINavigation alloc] initWithRootViewController:firstVC];
SecondViewController * secondVC = [[SecondViewController alloc] init];
//......
NSArray *viewControllers = [NSArray arrayWithObjects:firstVC,secondVC,nil];
UITabBarController*tabController = [[UITabBarController alloc] init];
tabController.viewControllers = viewControllers;
self.window.rootViewController = tabController;
```