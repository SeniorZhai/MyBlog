title: PendulemView
date: 2014-04-08 13:03:45
categories: iOS
tags: [iOS]
---
[PendulemView](https://github.com/zt1991616/PendulemView)
![](https://github.com/zt1991616/blog/raw/master/Image/14040801.gif)

```objective-c
  pendulum = [[PendulumView alloc] initWithFrame:self.view.bounds ballColor:ballColor ballDiameter:35];
    pendulum.hidesWhenStopped = NO;
    [self.view addSubview:pendulum];
```