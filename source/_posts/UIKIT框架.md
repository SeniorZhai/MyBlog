title: UIKIT框架
date: 2014-02-17 11:59:00
categories: iOS
tags: [iOS]
---
- UIView是视图的基类
- UIViewController视图控制器的基类
- UIResponder表示一个可以接受触摸屏上触摸事件的对象
- UIWin（窗口）是视图的一个子类，窗口的主要功能：1、提供一个区域来显示视图，2、将事件（event）分发给视图。
```Objective-c
	self.window = [[UIWindow alloc] initWithFrame:[UIScreen mainScreen] bounds];
	self.window.rootViewController = self.viewController;
	[self.window makeKeyAndVisible];
```

## UIScreen
    - UIScreen对象可以充当iOS设备物理屏幕的替代者，通过`[[UIScreen mainScreen] bounds]`可以获得设备的屏幕大小
    
## UIWindow
    - 通过UIApplication获取当前keyWindow，keyWindow是用来管理键盘以及触摸类的消息，并且只能有一个window是keyWindow.
    - UIWindow *keyWindow = [UIApplication sharedApplication].keyWindow;
    - 每个UIWindow对象配置windowLevel属性，大部分时候不应该去改变windowL.
    - UIWindow有3个级别，对应了3种显示优先级。通过windowLevel设置，优先级为：UIWindowLevel > UIWindowLevelStatusBar > UIWindowLevelNormal
```Objective-c
    //didFinishLauchingWithOptions
    self.windonw = [[[UIWindow alloc] initWithFrame:[[UIScreen mainScrren] bounds]];
    self.window.backgrondColor = [UIColor whiteColor];
    [self.window makeKeyAndVisible];
    NSLog(@"self.window level: %@",self.windonw.level);

    UIButton *startButton = [UIButton buttonWithType:UIButtonTypeRounedRect];
    startButton.frame = CCRectMake(320/2-120/2,180,120,35);
    [startButton setTile:@"警告" action:@selector(alertUser) forControlEvents:UIControlEventTouchUpInside];
    [self.window addSubview:startButton];
    
    return YES;
```
```Objective-c
    // alerUser
    -(void)alertUser
    {
        UIAlertView *alertView = [[UIAlertView alloc]
                                    initWithTitle:@"提示"
                                    message:@"警告框是alert Level级别的"
                                    delegate:nil
                                    cancelButtonTitle:@"确定"
                                    otherButtonTitles:nil];
        [alertView show];
    }
```