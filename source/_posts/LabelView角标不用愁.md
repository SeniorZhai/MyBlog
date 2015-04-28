title: LabelView角标不用愁
date: 2015-04-15 13:47:43
categories: Android
tags: [角标]
---
[LabelView](https://github.com/linger1216/labelview)是一款极其方便的角标视图，可以很容易地加载其他View上。
<!--more-->
##导入
```
dependencies {
    compile 'com.lid.labelview:lib:0.1.1'
}
```

##使用
`LabelView`继承至`TextView`，可以使用所有`TextView`的属性
```java
LabelView label = new LabelView(this);
label.setText("Text");
label.setBackgroundColor(0xff03a9f4);
label.setTargetView(mTv,10,LabelView.Gravity.LEFT_TOP);	// 指定在左上角
```
setTargetView的第二个参数是角标距角的位置
![](/img/15041501.png)

- 删除使用`remove`方法即可
