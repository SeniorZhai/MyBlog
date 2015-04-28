title: jQuery初识
date: 2015-03-30 20:15:28
categories: node.js
tags: [js,jQerry,前端]
---
<!--more-->
JQuery是一个JavaScript函数库，包含以下功能：
- HTML元素选取
- HTML元素操作
- CSS操作
- HTML事件函数
- JavaScript特效和动画
- HTML DOM遍历和修改
- AJAX
- Utilties
#jQuery安装
```html
<head>
	<script src="jquery-1.10.2.min.js" />
	<!-- <script src="http://libs.baidu.com/jquery/1.10.2/jquery.min.js"> -->
</head>
```

#语法
jQuery语法是通过选取HTML元素，并对选取的元素执行某些操作
##基础语法
`$(selector).action()`
- 美元符定义jQuery
- 选取符(selector)查询和查找HTML元素
- jQuery的action()执行元素的操作
###文档就绪事件
```js
$(document).ready(function(){
	// 为了防止文档在完全加载就绪之前运行jQuery代码
});
```
简洁的写法可以是这样的：
```js
$(function(){
	//
});
```
##选择器
jQuery选择器用于对HTML元素组或者单个元素进行操作
基于元素的id、类、类型、属性、属性值等"查找"HTML元素
所有选择器都以美元符开头：$()
- 基于元素名选取元素
```js
$(function(){
	$("button").click(function(){
		$("p").hide();
	})
})
```
- 基于id选择
```js
$(function(){
	$("button").click(function()){
		$("#test").hide();
	}
})
```
- 基于class选择
```js
$(function(){
	$("button").click(function(){
		$(".test").hide();
	});
})
```
###更多实例
|语法|描述|
|:--|:--|
|$("*")|所有元素|
|$(this)|当前元素|
|$("p.intro")|class为intro的`<p>`元素|
|$("p:first")|第一个`<p>`元素|
|$("ul li:first"|`<ul>`元素的第一个`<li>`元素|
|$("ul li:first-child"|每一个`<ul>`元素的第一个`<li>`元素|
|$("[href]")|选取所有带href属性的元素|
|$("a[target='_blank']")|所有target属性不等于`"_blank"`的`<a>`元素|
|$("a[target!='_blank']")|选取所有 target 属性值不等于 `"_blank"`的 `<a>`元素|
|$(":button")|选取所有 type="button" 的`<input>`元素和`<button>`元素|
|$("tr:even")|选取偶数位置的`<tr>`元素|
|$("tr:odd")|选取奇数位置的`<tr>`元素|

##jQuery事件
|鼠标事件|键盘事件|表单事件|文档/窗口事件|
|:---|:---|:---|:---|
|click|keypress|submit|load|
|dblclick|keydown|change|resize|
|mouseenter|keyup|focus|scroll|
|mouseleave||blur|unload|
###常用事件
- $(document).ready() 文档准备完毕
- click() 按钮点击事件
- dblclick() 双击元素事件
- mouseenter() 当鼠标指针穿过元素时
- mouseleave() 当鼠标离开元素时
- mousedown() 当鼠标移动到元素上并按下鼠标时
- mouseup() 当鼠标移动到元素上并松开鼠标时
- hover() 模拟光标悬停事件
- focus() 元素获得焦点事件
- blur() 元素失去焦点事件

###hide()和show()
用于元素的隐藏和显示
```js
$(selector).hide(speed,callback)	//speed 可以使slow fast或毫秒用于规定隐藏/显示的速度
$(selector).show(speed,callback)	
```
####toggle()
用于切换hide()和show()
```js
$(selector).toggle(speed,callback)
```
###淡入淡出效果
- fadeIn()
- fadeOut()
- fadeToggle()
- fadeTo()
```js
$(selector).fadeIn(speed,callback);
```
fadeIn为淡入，fadeOut为淡出，fadeToggle()为切换
fadeTo允许为给定的不透明度(值为0到1之间)
```js
$(selector).fadeTo(speed,opacity,callback)
```
###滑动效果
```js
$(selector).slideDown(speed,callback)	// 用于向下滑动元素
$(selector).slideUp(speed,callback)
$(selector).slideToggle(speed,callback)
```

##自定义动画
```js
$(selector).animate({params},speed,callback);
```
params为定义形成动画的CSS属性
```js
$("button").click(function(){
  $("div").animate({left:'250px'});
}); 
```
也可以同时操作多个属性
```js
$("button").click(function(){
  $("div").animate({
    left:'250px',
    opacity:'0.5',
    height:'150px',
    width:'150px'
  });
}); 
```
使用相对值
```js
$("button").click(function(){
  $("div").animate({
    left:'250px',
    height:'+=150px',
    width:'+=150px'
  });
}); 
```
使用预定值
```js
$("button").click(function(){
  $("div").animate({
    height:'toggle'
  });
}); 
```
使用队列
```js
$("button").click(function(){
  var div=$("div");
  div.animate({height:'300px',opacity:'0.4'},"slow");
  div.animate({width:'300px',opacity:'0.8'},"slow");
  div.animate({height:'100px',opacity:'0.4'},"slow");
  div.animate({width:'100px',opacity:'0.8'},"slow");
}); 
```

##停止动画
stop()方法用于停止动画或效果
`$(selector).stop(stopAll,goToEnd)`
可选的stopAll指定是否应该清除动画队列，默认是false，即仅停止活动的动画，允许任何排入队列的动画向后执行
可选的goToEnd参数指定是否立即完成当前动画，默认是false
```js
$("#stop").click(function(){
  $("#pancel").stop();
});
```