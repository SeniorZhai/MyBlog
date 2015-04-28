title: ShowTipsView提示说明控件
date: 2015-04-16 20:47:27
categories: Android
tags: [初始化,提示]
---
[ShowTipsView](https://github.com/fredericojssilva/ShowTipsView)是一款用于显示提示说明的控件
<!--more-->
##引用
```gradle
compile 'net.fredericosilva:showTipsView:1.0.1'
```

##使用
```java
ShowTipsView showtips = new ShowTipsBuilder(this)
    .setTarget(btn_test)
    .setTitle("A magnific button")
    .setDescription("This button do nothing so good")
    .setDelay(1000)
    .build();

showtips.show(this);

// 自定义属性
setTitleColor(int color)
setDescriptionColor(int color)
setBackgroundColor(int color)
setCircleColor(int color)

setTarget(View v, int x, int y, int radius)

showtips.setCallback(new ShowTipsInterface(){
    @Override
    public void gotItClicked() {
    //Lunch new showtip
    }
});
```

##示例
<https://github.com/SeniorZhai/ShowTipsView>