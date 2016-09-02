title: Masonry布局框架
date: 2015-04-15 17:10:59
categories: iOS
tags: [布局,Masonry,自动布局,自适应]
---
Masonry是一款轻量级的布局框架，采用优雅的链式语法封装了Apple的自动布局，具有极高的可读性和易用性。
<!--more-->

# Masonry支持的属性
```objective-c
@property (nonatomic,strong,readonly) MASConstraint *left;
@property (nonatomic,strong,readonly) MASConstraint *top;
@property (nonatomic, strong, readonly) MASConstraint *right;
@property (nonatomic, strong, readonly) MASConstraint *bottom;
@property (nonatomic, strong, readonly) MASConstraint *leading;
@property (nonatomic, strong, readonly) MASConstraint *trailing;
@property (nonatomic, strong, readonly) MASConstraint *width;
@property (nonatomic, strong, readonly) MASConstraint *height;
@property (nonatomic, strong, readonly) MASConstraint *centerX;
@property (nonatomic, strong, readonly) MASConstraint *centerY;
@property (nonatomic, strong, readonly) MASConstraint *baseline;
```

## 对应到NSLayoutAttrubute
|Masonry|NSLayoutAttriubute|说明|
|:---|:---|:---|
|left|NSLayoutAttributeLeft|左侧|
|top|NSLayoutAttributeTop|上侧|
|right|NSLayoutAttributeRight|右侧|
|bottom|NSLayoutAttributeBottom|下侧|
|leading|NSLayoutAttributeLeading|首部|
|trailing|NSLayoutAttributeTrailing|尾部|
|width|NSLayoutAttributeWith|宽|
|height|NSLayoutAttributeHeight|高|
|centerX|NSLyoutAttributeCenterX|横向中点|
|centerY|NSLyoutAttributteCenterY|纵向中点|
|baseline|NSLayoutAttributeBaseline|文本基准线|

###  居中显示View
```objective-c
- (void) viewDidLoad
{
	[super viewDidLoad];
	sv1.backgroundColor = UIColorFromRGB(0x434A54);
	UIView *sv = [UIView new];	
	[self.view addSubView:sv];
	[sv mas_makeConstraints:^(MASConstraintMaker *make) {
		make.center.equalTo(sv.superview);	// 中点与父View相同，即居中
		make.size.mas_equalTo(CGSizeMake(300,300));	
	}];
}
```

###  边距
```objective-c
UIView *sv = [UIView new];
[self.view addSubview:sv];
[sv mas_makeConstraints:^(MAConstraintMaker *make){
	make.edges.equalTo(sv.superview).with.insert(UIEdgeInsetsMake(10,10,10,10));
	/*
	make.top.equalTo(sv.superview).with.offset(10);
	make.left.equalTo(sv.superview).with.offset(10);
	make.bottom.equalTo(sv.superview).with.offset(-10);
	make.right.equalTo(sv.superview).with.offset(-10);
	*/
	/*
	make.top.left.bottom.and.right.equalTo(sv.superview).with.insert(UIEdgeInsetsMake(10,10,10,10));
	*/
}];
```
### 等宽并存在间隔
```objective-c
	UIView *sv3         = [UIView new];
    UIView *sv4         = [UIView new];
    sv3.backgroundColor = UIColorFromRGB(0x48CFAD);
    sv4.backgroundColor = UIColorFromRGB(0x4FC1E9);
    [sv2 addSubview:sv3];
    [sv2 addSubview:sv4];
    [sv3 mas_makeConstraints:^(MASConstraintMaker *make) {
        make.centerY.mas_equalTo(sv2.mas_centerY);  // 中心与superView相同，即居中
        make.left.equalTo(sv2.mas_left).with.offset(10);    //  左边与superView相同，间隔10
        make.right.equalTo(sv4.mas_left).with.offset(-10);  // 右边与sv4紧贴，间隔10
        make.height.mas_equalTo(@150);  // 高度150
        make.width.equalTo(sv4);
    }];
    [sv4 mas_makeConstraints:^(MASConstraintMaker *make) {
        make.centerY.mas_equalTo(sv2.mas_centerY);
        make.left.equalTo(sv3.mas_right).with.offset(10);
        make.right.equalTo(sv2.mas_right).with.offset(-10);
        make.height.mas_equalTo(@150);
        make.width.equalTo(sv3);
    }];
```
### UIScrollView顺序排列
```objective-c
UIScrollView *scrollView = [UIScrollView new];
    scrollView.backgroundColor = UIColorFromRGB(0xFFCE54);
    [self.view addSubview:scrollView];
    [scrollView mas_makeConstraints:^(MASConstraintMaker *make) {
        make.top.mas_equalTo(sv1.mas_bottom).offset(10);
        make.bottom.mas_equalTo(self.view.mas_bottom).offset(-10);
        make.left.mas_equalTo(sv1.mas_left);
        make.right.mas_equalTo(sv1.mas_right);
    }];
    
    UIView *container = [UIView  new];
    [scrollView addSubview:container];
    [container mas_makeConstraints:^(MASConstraintMaker *make) {
        make.edges.equalTo(scrollView);
        make.width.equalTo(scrollView);
    }];
    UIView *lastView = nil;
    for (int i = 0; i<10; ++i) {
        UIView *subView = [UIView new];
        subView.backgroundColor = [UIColor colorWithHue:( arc4random() % 256 / 256.0 )
                                             saturation:( arc4random() % 128 / 256.0 ) + 0.5
                                             brightness:( arc4random() % 128 / 256.0 ) + 0.5
                                                  alpha:1];
        [container addSubview:subView];
        [subView mas_makeConstraints:^(MASConstraintMaker *make) {
            make.left.and.right.equalTo(container);
            make.height.mas_equalTo(20);
            if ( lastView )
            {
                make.top.mas_equalTo(lastView.mas_bottom);
            }
            else
            {
                make.top.mas_equalTo(container.mas_top);
            }
        }];
        lastView = subView;
    }
    
    [container mas_makeConstraints:^(MASConstraintMaker *make) {
        make.bottom.equalTo(lastView.mas_bottom);
    }];
```