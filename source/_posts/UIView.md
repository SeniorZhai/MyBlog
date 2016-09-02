title: UIView
date: 2014-02-17 14:00:00
categories: iOS
tags: [iOS]
---
- 视图，表示屏幕上的一块矩形区域，同时处理该区域的绘制和触屏事件。
- 一个视图也可以作为其他视图的父视图，同时决定着这些子视图的位置和大小.
- UIView类做了大量的工作去管理这些内部视图的关系。
- 视图同时也是App中MVC的View部分
- iPhone的视图以左上角为原点
- 每个View的frame所使用的坐标系以它的父视图的左上角为坐
关函数
    + 视图结构和相
    ```Object_c
    CGPoint Point = CGPointMake(x,y);//设置
    CGSize size = CGSizeMake(width,height);//大小
    CGRect rect = CGRectMake(x,y,width,height);//位置和大小
    ```

## Frame和Bounds
- Frame以其父视图为起点，得出它自己的位置信息
- Bounds以iOS系统的坐标原点为起点，坐标是(0,0)
- Center表示视图中心所在的位置，设置此属性可改变视图的位置
    + 默认情况下，视图边框并不会被父视图的边框裁剪。如果需要裁剪，将其clipsToBounds属性设置为YES.
    
## 创建UIView
- 创建UI有两种方式，xib文件和代码创建

```Objective-c
    //通过xib方式来创建视图对象
    NSBundle *bundle = [NSBundle mainBundle];
    NSArray *arr = [bundle loadNibNamed:@"myView" owner:self
    options:nil];
    UIView *myView = [arr objectAtIndex:0];
```

```Objective-c
    //代码创建视图对象
    CGRect viewRect = CGRectMake(0,0,100,100);
    UIView *myView =[[UIView alloc] initWithFrame:viewRect];
```

## 视图的层次结构
- UIView层次结构可以理解为“视图树”————view hierarychy
- 一个视图就是一个容器，当一个视图包含其他的视图的时候，两个视图之间就建立了一个父子关系，被包含的视图被称为“姿势图（subView）”，包含的视图称为“父视图（superView）”
- 从视觉上看，子视图会覆盖父视图的内容，设置透明属性可以看到父视图的内容。
- 每个父视图都有一个有序的数组存储着它的子视图，存储的顺序就会影响到每个子视图的显示效果，后加的视图会覆盖之前的视图。
- 一个视图可以嵌入多个subView，但是只能有一个superView。
    + 视图的常用方法
    ```Objective-c
    addSubView:                     // 添加子视图
    insertSubview:atIndex:          // 视图插入到指定索引位置
    insertSubview:ahoveSubview:     // 视图插入指定视图之上
    insertSubview:belowSubview:     // 视图插入指定视图之下
    bringSubviewToFront:            // 把视图移动到最顶层
    sendSubviewToBack:              // 把视图移动到最底层
    exchangeSubViewAtIndex:withSubviewAtIndex://把两个索引对应的视图调换位置
    removeFromSuperview:            // 把视图从父视图中移除
    ```

## 查找视图
- UIView类中有一个tag属性，通过这个属性可以标志一个视图对象（整数）
- 获取的方法，viewWithTag:方法来检索标志过的子视图
    ```Objective-c
    UIView *myView = [[UIView alloc] initWithFrame:CGRectmake(0,0,100,100)];
    myView.tag = 100;
    // 通过tag查找view
    UIView *myView = [self.view vieWithTag:100];
    ```

## UIView的常用属性
- alpha                         // 透明度
- backgroundColor               // 背景颜色
- subViews                      // 子视图集合
- hidden                        // 是否隐藏
- tag                           // 标签值
- superview                     // 父视图
- mulitpleTouchEnaled           // 是否开启多点触摸
- userInteractionEnabled        // 是否响应触摸事件

##  坐标系统变换
- 坐标变换通过transform属性来改变
    + CGAffineTransformScale        对视图比例缩放
    + CGAffineTransformRotae        对视图做变焦旋转
    + CGAffineTransformTranslate    对视图相对原位置做平移
    ```Objective-c
    CGAffineTransform transform = rootView.transform;
    rootView.transform = CGAffineTransformScale(transform,0.5,0.5);
    rootView.transform = CGAffineTransformRotae(transform,0.33);
    CGAffineTransformScale(transform,0.5,0.5);
    rootView.transform = CGAffineTransformTranslate(transform,100,100);
    
## 视图的内容模式
- 视图的contentMode属性决定了边界变化和缩放操作
```Objective-c
    UIImageView *imgeView1 = [[UIImageView alloc] initWithFrame:CFRectMake(320/2-200/2,30,200,200)];
    imgeView1.imge = [UIImage imageNamed:@"01"];
    imgeView1.backgroundColor = [UIColor redColor];
    imgeView1.contentMode = UIViewContentModeScaleAspectFit;
    [self.window addSubview:imgeView1];
    [imView1 release];
    
    UIImageView *imgeView2 = [[UIImageView alloc] initWithFrame:CFRectMake(320/2-200/2,240,200,200)];
    imgeView2.backgroundColor = [UIColor yelloColor];
    imgeView2.contentMode = UIViewContentModeBottom;
    [self.window addSubview:imgeView2];
    [imView2 release];
```

## UIView属性的动画
- UIView类的很多属性都被设计为动画，动画的属性是指当属性从一个值变为另一个值的时候，可以半自动地支持动画，你仍然必须告诉UIKit希望执行什么类型的动画，但是动画一旦开始，Core Animation就会全权负责。UIView对象中支持动画的属性有如下几个：
    + frame - 动画的改变视图的尺寸和位置
    + bounds - 动画的改变视图的尺寸
    + center - 动画的改变视图的位置
    + transform - 动画的翻转或者缩放视x图
    + alpha - 动画的改变视图的透明度
    + backgroundColor - 改变视图的背景色
    + contentStetch - 改变视图内容如何拉伸
### 配置动画委托
    - 可以为动画分配一个委托，并通过该委托接受动画开始和结束的消息。当需要在动画开始前和动画结束后极力执行其他任务时，可能就需要设置委托。
    - 通过UIView调用setAnmationDelegate:方法来设置委托，并通过setAnimationWillStartSelector:和setAnimationDidStopSelector:方法来指定接受消息的选择器方法。消息处理方法形式如下:
    `(void)animationWillStart:(NSString *)animationID context:(void *)context;`
    `(void)animationDidStop:(NSString *)animationID finished context:(void *)context;`
    上面的两个方法的animationID和context参数和动画块开始时传给`beginAnimations:context:`方法的参数相同
        + animationID - 应用程序提供的字符串，用于标识一个动画块中的动画
        + context - 应用程序提供的对象，用于向委托对象传递额外的信息
        
    setAnimationDidStopSelector:选择器方法还有一个参数——即一个布尔值。如果动画顺利完成，没有被其他动画取消或停止，则该值为YES。
    ### 配置动画的参数
    - 用`setAnimationStartDateS`方法来设置动画在`commitAnimations:`方法返回之后的发生日期。
    - 用`setAnimationDelay:`方法来设置实际发生动画和`commitAnimations:`方法返回的时间点之间的间隔
    - 用`setAnimationDuration:`方法来设置动画的持续秒数
    - 用`setAnimationCurve:`方法来设置动画过程的相对速度，比如动画可能在启动阶段逐渐加速、而在结束阶段逐渐减少，或者这个过程都保持相同的速度
    - 用`setAnimationRepeatCount:`方法来设置动画的重复次数
    - 用`setAnimationRepeatAutoreverses:`方法来指定动画在到达目标值时是否自动反向播放。可是结合使用这个方法和`setAnimationRepeatCount:`方法，使各个属性在初始值和目标值之间平滑切换一段时间。
    - 缺省情况下，所有支持动画的属性在动画块中发生的变化都会形成动画。如果希望让动画块中发生的某些变化不产生动画效果，可以通过`setAnimationsEnableed:`方法来暂时禁止动画，在完成修改后才重新激活动画，在调用`setAnimationsEnabled:`方法并传入NO值之后，所有的改变都不会产生动画效果，指定用YES值再次调用这个方法或者提交这个动画块是，动画才会恢复，可以用`areAnimationsEnable:`方法来确定当前是否激活动画。
```Objective-c
    -(void)animationAlpha
    {
        [UIView beginAnimations:nil context:NULL];// 需要设置代理时
        [UIView setAnimationDuration:1];// 动画的持续时间
        [UIview setAnimationDelay:1];// 动画延迟时间
        view2.apleha = 0.0;
        [UIView commitAnimations];// 标记着动画块的结束
    }
    -(void)animationFrame
    {
        [UIView beginAnimations:nil context:NULL];
        [UIView setAnimationDuration:5];
        [UIView setAnimationCurve:UIViewAnimationCurveEaseOut];// 动画相对速度，开始和结束的时候慢，中间快
        view.center = CGPointMake(0,0);
        [UIView commitAnimations];
    }
```