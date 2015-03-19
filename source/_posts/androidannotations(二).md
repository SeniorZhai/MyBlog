title: androidannotations(二)
date: 2015-03-16 21:30:45
categories: Android
tags: [注入]
---
[androidannotations](https://github.com/excilys/androidannotations)是一款Android注入框架，可以方便我们编程，减少代码量(变相减少了错误的可能)，让我们可以更多的把精力放在逻辑处理上。
<!--more-->
##事件监听
点击事件
```java
@Click(R.id.myButton)
void myClick() {
  // ...
}

@Click
void anotherButton() {
  // ...
}

@Click
void yetAnotherButton(View clickedView) {
  // ...
}

@Click({R.id.myButton,R.id.myOtherButton}) 
void handlesTwoButtons(){
  // ...
}
```
其他事件
- 长按 @LongClick
- 触摸 @Touch
