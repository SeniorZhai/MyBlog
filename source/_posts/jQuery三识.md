title: jQuery三识
date: 2015-03-31 13:21:31
categories: node.js
tags: [js,jQerry,前端]
---
<!--more-->
##遍历DOM
###祖先
通过jQuery能够向上遍历DOM数，以查找元素的祖先
- parent() 	返回所选元素的直接父类
- parents() 返回所选元素的所有祖先元素，知道根元素(<html>)
- parentsUntil() 返回介于两个元素之间的所有祖先元素
```js
$(function(){
	$("span").prantsUntil("div");
})
```
###后代
jQuery能够向下遍历DOM树
- children() 返回被选元素的所有子元素
- find() 返回被选元素的后代元素，一路向下指导最后一个后代
###同胞
jQuery能够在DOM中遍历元素的同胞元素
- siblings() 返回被选元素的所有同胞元素
- next() 返回被选元素的下一个同胞元素
- nextAll() 返回被选元素的所有跟随的同胞元素
- nextUntil() 返回介于两个给定参数之间的所有跟随的同胞元素
- prev() 与next方向相反
- prevAll() 与nextAll方向相反
- prevUntil() 与nextUntil方向相反

##jQuery Ajax
AJAX = Asynchronous JavaScript and XML
简单的说在不重载整个页面的情况下，AJAX通过后台加载数据，并在网页上进行显示
###AJAX load
load()方法能够从服务器加载数据，并把返回的数据放入元素元素中
```js
$(selector).load(URL,data,callback);
```
必选参数URL为希望加载的URL，可选的data参数规定了请求一同发送的查询字符串的键/值对集合，可选的callback是load方法完成后所执行的函数
回调函数有不同的参数
- responseTxt 结果内容
- statusTxt 调用的状态
- xhr XMLHttpRequest对象
```js
$("button").click(function(){
  $("#div1").load("demo_test.txt",function(responseTxt,statusTxt,xhr){
    if(statusTxt=="success")
      alert("External content loaded successfully!");
    if(statusTxt=="error")
      alert("Error: "+xhr.status+": "+xhr.statusText);
  });
});
```
