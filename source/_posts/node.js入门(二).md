title: node.js入门(二)
date: 2014-07-01 17:22:41
categories: node.js
tags: [node.js]
---
##Express
Express是Node.js的一个Web框架，Web应用程序有公用的模式，所以使用框架来构建经常事半功倍。其他一些成熟的框架，比如：
- Ruby on Rails(Ruby)
- Sinatra(Ruby)
- Django(Python)
- Zend(PHP)
- CodeIgniter(PHP)

一些使用Express能做的事情有：
- 基于JSON的API
- 单页面Web应用程序
- 事实Web应用程序

安装Express：`npm install -g express`
使用`-g`标记表示Express将是全局安装。

Express项目文件结构如下：
- app.js
app.js是用来启动应用程序的应用程序文件夹，它包含应用程序的配置信息。
- node_modules
用来保存在pack.json中定义并且已经安装的Node模块
- package.json
提供应用程序的信息，包括运行应用程序所需安装的依赖模块
- public
Web进行服务的公共文件夹，在这个文件夹中可以看到样式单、javascript和图片
- routes
路由，定义了应用程序应该响应的页面
- views
视图文件夹定义应用程序的布局
在views文件夹有很多以`.jade`为扩展名的文件，Express利用模板引擎将视图编译成HTML，默认情况，Express使用Jade作为模板引擎。
###Jade
Jade通过缩进定义页面结构，如果一行在上一行下面缩进，就认为它是上一行的子行。
- html 编译后是： <html></html>
这里可以使用任何HTML标记(body、section、p等等)
- 要给标记使用id，可加上一个`#`号后跟id名称，注意不允许有空格
section#wrapper 编译后： <section id="wrapper"></section>
- 可以添加一个类，方法是加上一个小数点后跟类的名称。
p.highlight 编译后： <p class="highlight"></p>
- 急需要类也需要id
section#wrapper.class-name 编译后： <section id="wrapper" class="class-name"></section>
- Jade也支持在一个标记上有多个类
p.first.second.third.fourth 编译后： <p class="first second third fourth">
- 为了创建HTML结构，要使用缩进
```
p
    span
```
编译后：
```html
<p><span></span></p>
```
要标记中加入文本，只需在标记定义后加入即可
h1 Very important heading 编译后： <h1>Very important heading</h1>
- Jade支持管道描述符(`|`)组织大文本
```
p
    | Text can be over
    | many lines
    | after a pipe symbol
```
编译后：
```html
<p>Text can be over many lines after a pipe symbol</p>
```
###Jade输出数据
Jade使用两个特殊符号来决定应该如何翻译代码，当一个字符是减号(-)，用于告诉随后的代码应当被执行。第二个字符是字符(=)告诉解释器要对对代码进行演算、转义，然后输出。
####变量
- -var foo = bar
变量设置之后可以再以后使用：
p I want to learn to use variables.Foo is #{foo}!
”#{变量}“这个特殊语法将用变量来代替：
<p>I want to learn to use variables.Foo is bar!</p>
####循环
循环用来迭代数组和对象。

```
- users = ['Sally','Joseph','Michael','Sanjay']
- each user in users
  p= user
```
编译后：
```
<p>Sally</p>
<p>Joseph</p>
<p>Michael</p>
<p>Sanjay</p>
```
也可以使用`for`关键字

```
- for user in users
  p= user
```

对象进行迭代：

```
- obj = {first_name:'George',surname:'Ornbo'}
- each val,key in obj
  li #{key}:#{val}
```   
编译后：
```
<li>first_name:George</li>
<li>surname:Ornbo</li>
```
####条件
```
- awake = false
- if (awake)
 p You are awake! Make coffee!
- else
 p You are sleeping
```
`if`和`else`关键字之前的减号(-)是可选的
####内联(inline)JavaScript
```
script
    alert('You can execute inline JavaScript through Jade')
```
#####包含(include)
Jade通过include关键字后跟想要包含的模板来支持包含功能
```
html
    body
        include includes/header
```
