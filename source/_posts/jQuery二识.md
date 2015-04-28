title: jQuery二识
date: 2015-03-31 13:21:31
categories: node.js
tags: [js,jQerry,前端]
---
<!--more-->
##jQuery DOM操作
DOM = Document Object Model(文档对象模型)
jQuery提供了一系列与DOM相关的方法，这使访问和操作元素和属性变得容易

##获取HTML元素

###获得内容
- text() 设置或返回所选元素的文本内容
- html() 设置或返回所选元素的内容(包括HTML标记)
- val() 设置或返回表单字段的值
以上的三个都拥有回调函数，由两个参数：被选元素列表中当前元素的下标以及原始值

###获取属性
- attr() 获取属性值
设置属性
```js
$("button").click(function(){
	$("#w3s").attr("href","http://www.baidu.com/");
});
```
同事设置多个属性
```js
$("button").click(function(){
	$("#w3s").attr({
		"href":"http://www.sina.com.cn",
		"title":"Sina"
	})
})
```
attr()也通过回调函数，包含两个参数：被选元素列表中当前元素的下标，以及原始值
```js
$("button").click(function(){
	$("#w3s").attr("href",function(i,origValue){
		return origValue + "/jquery";
	});
})
```

###添加元素
- append()	在被选元素的结尾插入内容
- prepend()	在被选元素的开头插入内容
- after()	在被选元素之后插入内容
- before()	在被选元素之前插入内容
```js
$("p").addend("Some appended text.")
```

###删除元素
- remove()	删除被选元素(以及子元素)
- empty()	从被选元素中删除子元素
remove()方法可以接受一个参数，允许对删除元素进行过滤
```js
$("p").remove(".italic");	// 删除class="italic"的所有<p>元素
```

##获取并设置CSS类

###操作CSS
- addClass()	向所选元素添加一个或多个类
- removeClass()	从被选元素删除一个或多个类
- toggleClass()	对被选元素进行添加/删除类的切换操作
- css()			设置或返回样式属性

```js
$("button").click(function(){
	$("h1,h2,p").addClass("blue");
	$("div").addClass("important");
	// $("div").addClass("important blue"); 添加多个类
	$("div").removeClass("blue");
	$("div").toggleClass();
})
```

####css()方法
`css("propertyname")`
```js
$("p").css("background-color");
$("p").css("background-color","yellow"); //设置属性
$("p").css({"background-color":"yellow","font-size":"200%"});	// 同时设置多个属性
```

##尺寸
![](/img/15033101.gif)
- width()		设置或返回元素的宽度(不包含内边距，边框或外框)
- height()		设置或返回元素的高度(不包含内边距，边框或外框)
- innerWidth()	设置或返回元素的宽度(包含内边距)
- innerHeight()	设置或返回元素的高度(包含内边距)
- outerWidth()	设置或返回元素的宽度(包含内边距和边框)
- outerHeight()	设置或返回元素的高度(包含内边距和边框)
