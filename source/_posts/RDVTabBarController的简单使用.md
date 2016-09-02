title: RDVTabBarController的简单使用
date: 2015-04-27 21:04:57
categories:
tags:
---
[RDVTabBarController](https://github.com/robbdimitrov/RDVTabBarController)是一款高度定制化的TabBarController，可以方便的对每一个TabItem进行配置。
<!--more-->
## 使用
在pod中添加
```
pod 'RDVTabBarController', '~> 1.1.9'
```
## 初始化
```objective-c
UIViewController *firstViewController = [[FirstViewController alloc] init];
UIViewController *firstNavigationController = [[UINaigationController alloc] initWithRootViewController:firstViewController];

UIViewController *secondViewController = [[SecondViewController alloc] init];
UIViewController *secondNavigationViewController = [[UINaigationController alloc] initWithRootViewController:sencondViewController];

UIViewController *thirdViewController = [[ThirdViewController alloc] init];
UIViewController *thirdNavigationViewController = [[UINaigationController alloc] initWithRootViewController:thirdViewController];

RDVTabBarController *tabBarController = [[EDVTabBarController alloc] init];
[tabBarController setViewControllers:@[firstNavigationController,secondNavigationViewController,thirdNavigationViewController]];

self.viewController = tabBarController;
```

## 定制化TabItem
```objective-c
UIImage *finishedImage = [UIImage imageNamed:@"selected_background"];
UIImage *unfinishedImage = [UIImage imageNamed:@"normal_background"];
[[tabBarController tabBar] items];

```