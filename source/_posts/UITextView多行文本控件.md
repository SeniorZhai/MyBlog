title: UITextView多行文本控件
date: 2014-02-22 14:39:44
categories: iOS
tags: [iOS]
---
- 继承了UIScrollView:UIView控件，默认带有滚动条
- UITextView与UITextField的区别
	+ UITextView是一个多行文本框，UITextField只是单行文本
	+ UITextView没有继承UIControl控件
	+ UITextView继承了UIScrollView，所以它具有UISrollView的功能和行为。
##  UIScrollView支持的属性
- 控制显示区域的属性
	+ contentSize:该属性是一个CGSize类型的值，制定显示内容的完整宽度和完整高度。
	+ contentInset:该属性是一个UIEdgeInsets类型的值，表示所需要内容在上下左右的留白。
	+ contentOffset:该属性是一个CGPoint值，表示可视区域在显示内容上滚动的距离。
- 属性设置
1.Scrollers
	+ Shows Horizontal Scrllers:当用户水平滚动该控件时，该控件会显示水平滚动条。
	+ Shows Vertical Scrollers: 当用户垂直滚动该控件时，该控件会显示垂直滚动条。
	+ Scrolling Enabled:控件能否滚动。
	+ Paging Enabled:是否分页。
	+ Direction Lock Enabled:第一次滚动后，不允许向其他方向滚动。
2. Bounce
	+ Bounce :弹回效果。
	+ Bounce Horizontally:水平弹回效果。
	+ Bounce Vertically:垂直弹回效果。
3. Zoom
	+ Min 最小的可缩放比例。
	+ Max 最大的可缩放比例。
4. Touch
	+ Bounces Zoom:内容缩放是否有弹性。
	+ Delays Content Touches:真正确定滚动意图才去处理触碰手势。
	+ Cancellable Content Touches:勾选后，如果该UIScrollView中的内容已经跟踪用户手势触碰动作的情况下，且用户拖动手指足以启动一个滚动事件，该UISrollView控件会调用touchesCancelled:withEvent:方法，并将该手指拖动事件当成滚动该UISroll控件。如果不勾选，UIScrollView控件已经跟踪某个手指动作，将不会理会其他的手指动作。
- 使用委托对象处理UITextView事件
	+ 委托对象必须实现UITextViewDelegate协议，协议定义了如下方法。
	1. -textViewShouldBeginEdting:将要开始编辑内容时回调。
	2. -textViewDidBeginEdting:开始编辑内容时回调。
	3. -textViewShouldEndEdting:将要结束时回调。
	4. -textViewDidEndEdting:结束编辑时回调。
	5. -textView:shouldChangeTextInRange:replacementText:指定范围内的文本内容将要被替换是回调。
	6. -textViewDidChange:文本内容发生改变时回调。
	7. -textViewDidChangeSelection:文本内某些文本改变时回调。
- [实例](https://github.com/zt1991616/iOSDemo/tree/master/UIViewDemo6)