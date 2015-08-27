title: Swift版Masonry-SnapKit布局框架
date: 2015-08-26 23:00:38
categories:
tags:
---
和[Masonry](http://seniorzhai.github.io/2015/04/15/Masonry%E5%B8%83%E5%B1%80%E6%A1%86%E6%9E%B6/)一样，SnapKit是一套轻量级的布局框架，同样适用链式语法封装Apple的自动布局约束。
<!--more-->
##导入
```
platform :ios, '7.0'
use_frameworks!

pod 'SnapKit', '~> 0.12.0'
```
##约束对应的属性
|ViewAttribute|NSLayoutAttriubute|说明|
|:---|:---|:---|
|view.snp_left|NSLayoutAttributeLeft|左侧|
|view.snp_top|NSLayoutAttributeTop|上侧|
|view.snp_right|NSLayoutAttributeRight|右侧|
|view.snp_bottom|NSLayoutAttributeBottom|下侧|
|view.snp_leading|NSLayoutAttributeLeading|首部|
|view.snp_trailing|NSLayoutAttributeTrailing|尾部|
|view.snp_width|NSLayoutAttributeWith|宽|
|view.snp_height|NSLayoutAttributeHeight|高|
|view.snp_centerX|NSLyoutAttributeCenterX|横向中点|
|view.snp_centerY|NSLyoutAttributteCenterY|纵向中点|
|view.snp_baseline|NSLayoutAttributeBaseline|文本基准线|

##居中
```swift
	let view1 = UIView()
    view1.backgroundColor = getColor(0x6FBFAC)
    view.addSubview(view1)
    view1.snp_makeConstraints{(make)->Void in
        make.edges.equalTo(view).insets(UIEdgeInsetsMake(20, 20, 20, 20))
    }
```

##等宽
```swift
let view2 = UIView()
let view3 = UIView()
        
view2.backgroundColor = getColor(0xE5395F)
view3.backgroundColor = getColor(0x402516)
        
        
view1.addSubview(view2)
view1.addSubview(view3)
view1.addSubview(view4)
view2.snp_makeConstraints{(make)->Void in
    make.top.equalTo(view1.snp_top).offset(20)
    make.width.equalTo(view3)
    make.left.equalTo(view1.snp_left).offset(20)
    make.right.equalTo(view3.snp_left).offset(-20)
    make.height.equalTo(200)
}
        
view3.snp_makeConstraints{(make)->Void in
    make.top.equalTo(view1.snp_top).offset(20)
    make.width.equalTo(view2)
    make.right.equalTo(view1.snp_right).offset(-20)
    make.left.equalTo(view2.snp_right).offset(-20)
    make.height.equalTo(200)
}
```

###相对在下
```swift
view4.snp_makeConstraints{(make)->Void in
    make.width.equalTo(view1).offset(-40)
    make.centerX.equalTo(view.snp_centerX)
    make.top.equalTo(view2.snp_bottom).offset(20)
    make.bottom.equalTo(view1.snp_bottom).offset(-20)
}
```

在使用自动布局的时候只要记住View需要大小、间隔、位置(中心点、X、Y)常见的布局都可以完成
例子可以在<https://github.com/SeniorZhai/SnapDemo>看到
更多用法可以参考[文档](http://snapkit.io/docs/)