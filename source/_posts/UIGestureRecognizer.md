title: UIGestureRecognizer
date: 2014-04-30 13:07:17
categories: iOS
tags: [iOS]
---
- UITapGestureRecognizer（点一下）
- UIPinchGestureRecognizer（两指往内或往外拨动，就是常用的缩放）
- UIRotationGestureRecognizer（旋转）
- UISwipeGestureRecognizer（滑动）
- UIPanGestureRecognizer（拖动，慢速移动）
- UILongPressGestureRecognizer（长按）

UIGestureRecognizer的继承关系如下：

![](https://github.com/zt1991616/blog/raw/master/Image/14043001.jpg)

##使用手势的步骤
1. 创建手势实例，指定一个回调方法，当手势开始，改变或结束时，回调方法被调用。
2. 添加到需要识别的View中，每个手势只能对应一个View，当屏幕触摸在View的边界内，如果手势和预定的一样，就会回调方法。

ps:一个手势只能对应一个View，但一个View可以对应多个手势。

###Pan拖动手势
```objective-c
UIImageView *snakeImageView = [[UIImageView alloc] initWithImage:[UIImage imageName:@"snake.png"]];
snakeImageView.frame = CGReckMake(50,50,100,160);
UIPanGestureRecognizer *panGestureRecongnizer = [[UIPanGestureRecoginizer alloc] initWithTarget:self action:@selector(handlePan:)];
[snakeImageView addGestureRecognizer:panGestureRecongnizer];
[self.view setBackgroundColor:[UIColor whiteColor]];
[self.view addSubview:snakeImageView];
```
添加手势回调方法
```objective-c
- (void) handlePan:(UIPanGestureRecognizer*) recognizer
{
	CGPoint translation = [recognizer translationInView:self.view];
	recognizer.view.center = CGPointMake(recognizer.view.center.x + translation.x,recognizer.view.center.y + translation.y);
	[recognizer setTranslation:CGPointZero inView:self.view];
}
```

###Pinch缩放手势
```objective-c
UIPinchGestureRecoginzer *pinchGestureRecognizer = [[UIPinchGestureRecognizer alloc] initWithTarget:self action:@selector(handlePinch:")];
[snakeImageView addGestureRecognizer:pinchGestureRecognizer];

- (void)handlePinch:(UIPinchGestureRecongnizer*) recognizer
{
	recognizer.view.transform = CGAffineTransformScale(recognizer.view.transform,recognizer.scale, recognizer.scale);
	recognizer.scale = 1;
}
```

###Rotation旋转手势

```objective-c
UIRotationGestureRecognizer *rotateRecognizer = [[UIRotationGestureRecognizer alloc]  initWithTarget:self action:@selector(handleRotate:)];  
[snakeImageView addGestureRecognizer:rotateRecognizer];

- (void) handleRotate:(UIRotationGestureRecognizer*) recognizer  
{  
    recognizer.view.transform = CGAffineTransformRotate(recognizer.view.transform, recognizer.rotation);  
    recognizer.rotation = 0;  
}
```

###多个View添加手势
```objective-c
- (void)viewDidLoad  
{  
    [super viewDidLoad];  
 
    UIImageView *snakeImageView = [[UIImageView alloc] initWithImage:[UIImage imageNamed:@"snake.png"]];  
    UIImageView *dragonImageView = [[UIImageView alloc] initWithImage:[UIImage imageNamed:@"dragon.png"]];  
    snakeImageView.frame = CGRectMake(120, 120, 100, 160);  
    dragonImageView.frame = CGRectMake(50, 50, 100, 160);  
    [self.view addSubview:snakeImageView];  
    [self.view addSubview:dragonImageView];  
 
    for (UIView *view in self.view.subviews) {  
        UIPanGestureRecognizer *panGestureRecognizer = [[UIPanGestureRecognizer alloc] initWithTarget:self action:@selector(handlePan:)];  
 
        UIPinchGestureRecognizer *pinchGestureRecognizer = [[UIPinchGestureRecognizer alloc] initWithTarget:self action:@selector(handlePinch:)];  
 
        UIRotationGestureRecognizer *rotateRecognizer = [[UIRotationGestureRecognizer alloc] initWithTarget:self  action:@selector(handleRotate:)];  
 
        [view addGestureRecognizer:panGestureRecognizer];  
        [view addGestureRecognizer:pinchGestureRecognizer];  
        [view addGestureRecognizer:rotateRecognizer];  
        [view setUserInteractionEnabled:YES];  
    }  
    [self.view setBackgroundColor:[UIColor whiteColor]];       
}
```

###监听手势
```objective-c
- (void) handlePan:(UIPanGestureRecognizer*) recognizer  
{  
    CGPoint translation = [recognizer translationInView:self.view];  
    recognizer.view.center = CGPointMake(recognizer.view.center.x + translation.x,  
                                       recognizer.view.center.y + translation.y);  
    [recognizer setTranslation:CGPointZero inView:self.view];  
 
    if (recognizer.state == UIGestureRecognizerStateEnded) {  
 
        CGPoint velocity = [recognizer velocityInView:self.view];  
        CGFloat magnitude = sqrtf((velocity.x * velocity.x) + (velocity.y * velocity.y));  
        CGFloat slideMult = magnitude / 200;  
        NSLog(@"magnitude: %f, slideMult: %f", magnitude, slideMult);  
 
        float slideFactor = 0.1 * slideMult; // Increase for more of a slide  
        CGPoint finalPoint = CGPointMake(recognizer.view.center.x + (velocity.x * slideFactor),  
                                         recognizer.view.center.y + (velocity.y * slideFactor));  
        finalPoint.x = MIN(MAX(finalPoint.x, 0), self.view.bounds.size.width);  
        finalPoint.y = MIN(MAX(finalPoint.y, 0), self.view.bounds.size.height);  
 
        [UIView animateWithDuration:slideFactor*2 delay:0 options:UIViewAnimationOptionCurveEaseOut animations:^{  
            recognizer.view.center = finalPoint;  
        } completion:nil];  
 
}
```