title: 常用UIView组件
date: 2014-02-18 12:46:08
categories: iOS
tags: [iOS]
---
## UIButton
- 常用属性

1.Type
    + Custom:选择此属性外观主要依靠自定义实现。
    + System:系统能够默认风格。
    + Detail Disclosure:详情标示。
    + Info Light:显示“i”图标的图形按钮。
    + IndoDark:显示“i”图标的图形按钮。
    + Add Contact:显示黑色“+”图标图形按钮。
2. State Config
    - 按钮的状态
        + 默认状态（Default）
        + 高亮状态（Highlighted）
        + 选中状态（Selected）
        + 禁用状态（Disable）
3. Title
    - 可以选择Plain和Attributed文本方式
    - 制定显示的字符信息
4. Font
    - 文本字体、大小、字体风格
5. Text Color
    - 文本的颜色
6. Shadow Color
    - 阴影颜色
7. Image
    - 设置为图片按钮，Title属性将不会起作用。
8. Background
    - 设置背景图片
9. Shadow Offset
    - 文本和阴影的偏移量
10. Link Break
    - 省略方式
11. Edge
    - Content:内容作为边界
    - Title:文本作为边界
    - Image:图片作为边界
12. Inset
    - 边界距离

## UIAlert And UIActionSheet
```Objective-C
    UIAlertView *alertView = [[UIAlertView alloc] initWithTitle:@"title" message:@"message" delegate:nil cencelButtonTitle:@"cancel" otherButtonTitles:@"other",nil];
    [alertView show];
```
```Objective-C
    UIActionSheet *actionSheet = [[UIActionSheet alloc] initWIthTitle:@"title" delegate:nil cancelButtonTitle:@"cancel"
    destructiveButtonTitle:@"destrutive"
    otherButtonTitles:@"other1",nil];
    [actionSheet show];
```

## UIButton
- UIButton
    + 作用：响应用户的点击事件的View
- 常用方法
```Objective-C
// 设置指定状态对应的标题文本
-(void)setTitle:(NSString *)title forState:(UIControlState)state;
// 设置指定状态对应的标题颜色
-(void)setTitleColor:(UIColor *)color forState:(UIControlState)state;
// 设置指定状态对应的显示图片
-(void)setImage:(UIImage *)image forState:(UIControlState)state;
// 设置指定状态对应的背景图片
-(void)setBackgroundImage:(UIImage *)image forState:(UIControlState)state;
// 为按钮添加事件
-(void)addtarget:(id *)target action:(SEL)action forControlEvents:(UIControlEvents)controlEvents;
```
- UIButton状态
```Objective-C
UICtrolStateNormal          // 正常状态
UICtrolStateHighlighted     // 高亮状态
UICtrolStateDisabled        // 禁用状态
UICtrolStateSelected        // 选中状态
UICtrolStateApplication
UICtrolStateReserved
```

- 事件处理

```Objective-C
// 用户按下时触发
UIControlEventTouchDown
// 点击计数大于1的触发
UIControlEventTouchDownRepeat
// 当触摸在控件内部拖动时触发
UIControlEventTouchDragInside
// 当触摸在控件之外拖动时触发
UIControlEventTouchDragDutside
// 当触摸从控件之外拖到内部时
UIControlEventTouchDragEnter
// 当触摸从控件内部拖到外部时
UIControlEventTouchDragExit
// 控件之内触摸抬起时
UIControlEventTouchUpInside
// 控件之外触摸抬起时
UIControlEventTouchUpOutside
// 触摸取消时间，设备被上锁或者电话呼叫打断
UIControlEventTouchCanel
```

- 代码示例

```Objective-C
    UIButton *button = [UIButton buttonWithType:UIButttonTypeRoundedRect];
    [button setTitle:@"Normal" forState:UIControlNormal];
    [button setTitle:@"Highlighted" forState:UIControlHighlighted];
    [button setTitle:@"Disabled" forState:UIControlDisabled];
    [button setTitle:@"Selected" forState:UIControlSelected];
    button.frame = CGRectMake(90,100,140,40);
    [self.window.addSubview:button];
```
## 图片视图
- UIImageView
    + 作用：专用于显示图片的视图

- 常用属性和方法
```Objective-c
// 初始化图片
-(id)initWithImage:(UIImage *)image;
// 初始化高亮的图片
-(id)initWithImage:(UIImage *)image hightlightedImage:(UIImage *)highlightedImage
// 点语法设置图片
@property(nonatomic,retain)UIImage *image;
// 点语法设置高亮图片
@property(nonatomic,retain)UIImage *highlightedImage;
// 是否打开用户交互，默认为NO
@preperty(nonatomic,getter=isUserInteractionEnabled)BOOL userInteractionEnabled;
```
- 示例代码
```Objective-C
    UIImageView *imageView = [[UIImageView alloc] initWithFrame:CGRectMake(0,0,100,100)];
    imageView.image = [UIImage imageNamed:@"t.png"];
    imageView.highlightImage = [UIImage imageNamed:@"t1.png"];
    imageView.userInteractionEnabled = YES;
```
## UIActivityIndicatorView
- UIActivityIndicatorView
    + 作用：提示用户当前页面正在加载数据
- 常用属性和方法
```Objective-C
// 设置风格
@property(nonatomic)UIActivityIndicatorViewStylr;
// 停止时、隐藏视图，默认为YES
@property(nonatomic)BOOL hideWhenStopped;
// 修改颜色、注意版本问题
@property(readWrite,nonatomic,retain)UIColor *color;
// 开始动画
-(void)startAnimating;
// 停止动画
-(void)stopAnimating;
// 判断动画的状态（停止或开始）
-(BOOL)isAnimating;
```
- 示例代码
```Objective-C
    UIActivityIndicatorView *activityView = [[UIActivityIndicatorView alloc] initWithActivityIndicatorViewStyleWhite];
    activityView.frame = CGRectMake(0,0,100,100);
    [activityView startAnimating];
```

## 滑动控件
- UISlider视图
    + 作用：控制系统声音，或者表示播放进度等等
- 常用属性
```Objective-C
// 设置获取slider的value值
@property(nonatomic)float value;
// 设置slider的最小值
@property(nonatomic)float minimumValue;
// 设置slider的最大值
@property(nonatomic)float maximumValue;
// 设置图片
@property(nonatomic,retain)UIImage *minimumValueImage;
// 设置图片
@property(nonatomic,retain)UIImage *maximumValueImage;
// 设置slider的value值，是否存在动画
-(void)setValue:(float)value animated:(BOOL)animated;
```
- 示例代码
```Objective-C
    UISlider *slider = [[UISlider alloc] initWithFrame:CGRectMake(20,0,150,25)];
    [slider addTarget:self action:@selector(sliderAction:) forControlEvent:UIControlEventValueChanged];
    slider.maxmumValue = 100;
    slider.minmumValue = 0;
    slider.value = 50;
```

## 分段控件
- UISegmentedControl
    + 作用：分段控件、页面切换等
- 示例代码
```Objective-C
    NSArray *array = [NSArray arrayWithObjects:@"选择",@"搜索",@"工具",nil];
    UISegmentedControl *segmentCtrl = [[UISegmentedControl alloc] initWithItems:array];
    segmentCtrl.frame = CGRectMake(20,0,150,25);
    segmentCtrl.segmentedControlStyle = UISegmentedControlStyleBar;
    segmentCtrl.selectedSegmentIndex = 0;
    [segmentCtrl addTarget:self action:@selector(segmentAction:) forControlEvents:UIControlEventValueChanged];
```

## 分页控件
- UIPageControl
    + 作用：通常和UIScrollView连用，提示用户当前显示页数
- 常用属性和方法
```Objective-C
// 共有几个分页“圆圈”
@property(nonatomic)NSInteger numberOfPages;
// 显示当前的页
@property(nonatomic)NSInteger currentPage;
// 只存在一页时，是否隐藏，默认为YES
@property(nonatomic)BOOL hidesForSinglePage;
// 刷新视图
-(void)updateCurrentPageDisplay;
```
- 示例代码
```Objective-C
    UIPageControl *pageControl = [[UIPageControl alloc]initWithFrame:CGRectMake(0,100,320,40)];
    pageControl.numberOfPages = 10;
    pageControl.currentPage = 2;
    pageControl.backgroundColor = [UIColor grayColor];
    [pageControl addTarget:self action:@selector(change:) froControlEvents:UIControlEventValueChanged];
```

## UITextField
- 常用代理方法
```Objective-C
// 将要开始输入时调用
-(BOOL)textFieldShoudBeginEdting:(UITextField *)textField{
    return YES;
}
// 将要输入结束时调用
-(BOOL)textFieldShouldEndEditing:(UITextField *)textField{
    return YES;
}
// 清除文字按钮点击事件
-(BOOL)textFieldShouldClear:(UITextField *)textField{
    return YES;
}
// 键盘上的return按钮
-(BOOL)textFieldShouldReturn:(UITextField *)textField{
    [textField resignFirstResponder];
    return YES;
}
```
## UILabel
- UILabel
    + 作用：显示文本
- 常用属性
```Objective-C
// 设置文本内容，默认为nil
@property(nonatomic,copy)NSString *text;
// 设置字体大小
@property(nonatomic,retain)UIFont *font;
// 设置字体颜色
@property(nonatomic,retain)UIColor *textColor;
// 设置阴影，默认没有阴影，若果设置需要设置偏移量
@property(nonatomic,retain)UIColor *shadowColor;
// 设置偏移量
@property(nonatomic）CGSize shadowOffset;
// 文本内容对齐方式
@property(nonatomic)NSTextAlignment textAlignment;
// 当文本超出frame时，文本截取的方式
@property(nonatomic)NSLineBreakMode lineBreakMode;
// 文本选中时，高亮的颜色
@property(nanatomic,retain)UIColor *highlightedTextColor;
// 是否存在高亮，默认为nil
@property(nanatomic,getter=isHighlighted)BOOL highlighted;
// 交互是否打开，默认为NO
@property(nanatomic,getter=isUserInteractionEnabled)BOOL userInteractionEnabled;
```
- 示例代码
```Objective-C
    UILabel *label = [[UILabel alloc]initWithFrame:CGRectMake(90,100,140,40)];
    label.text = @"text";
    label.backgroundColor = [UIColor redColor];
    label.textAligment = NSTextAligmentCenter;
    label.textColor = [UIColor blueColor];
    label.shadowColor = [UIColor yellowColor];
    label.shadowOffset = CGSizeMake(-2,2);
    [self.window addSubview:label];
    [label release];
```