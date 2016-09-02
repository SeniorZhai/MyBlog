title: node.js入门(一)
date: 2014-07-01 11:22:28
categories: node.js
tags: [node.js]
---
用于Windows和OS X的安装程序可以再Node.js的主页下载:[http://nodejs.org/](http://nodejs.org)

## 验证Node.js是否正确安装
在命令行中输入`node`可以看到以下提示:

![](https://github.com/zt1991616/blog/raw/master/Image/14063001.png)

![](https://github.com/zt1991616/blog/raw/master/Image/14070801.png)

## First Code
```javascript
// server.js
var http = require('http')
http.createServer(function (req,res){
    res.writeHead(200,{'Content-Type':'text/plain'});
    res.end('Hello Node.js\n');
}).listen(3000,"127.0.0.1");
console.log("Server running at http://128.0.0.1:3000/");
```
在命令行中执行`node server.js`，一个简单的web吴福气就完成了

## npm

npm(Node Package Manager)是Node.js的包管理器，它允许开发者在Node.js应用程序中创建、共享并重用模块。

- 安装模块
`npm install [module_name]`

- 使用模块
下载之后必须请求(require)它们，`var module = require('module');`

- 如何找模块
    1. 官方来源：在`http://search.npmjs.org/`上有一个官方基于Web的npm搜索工具。也可以在nmp命令行工具来搜索Node.js模块`npm search irc`
    2. 非官方来源：如`http://blago.dachev.com/modules`站点，该站点可以提供了Github上某个项目的围观者数量、分叉(fork)数量以及问题数量。或者`http://eirikb.github.com/nipster/`根据GitHub上的分叉数量对项目评级。

- 本地和全局的安装
    1. 本地安装：本地安装意味着库将安装在项目本地的一个名为`node_modules`的文件夹下以便项目使用，这是默认行为。
    ├ foo.js
    ├ node_modules/module_name
    2. 全局安装：有些模块可能任何一个位置都能运行这些可执行文件，需要全局安装，只需要在安装时加上-g标记。`npm install -g express`
- 查找模块文档
    通过`npm docs [module_name]`一般就能查看文件，还可以使用`npm bugs [module_name]`查看项目的Bug
- 使用package.json指定依赖关系(dependency)
    npm允许开发人员使用`package.json`文件来指定应用程序中要用的模块，并且通过单个命令来安装它们`npm install`，如下例
    1. 创建一个模块foo.js
        ```javascript
        var _ = require('underscore');
        _.each([1,2,3],function(num){
            console.log("underscore.js says " + num);
        });
        ```
    2. 创建一个package.json
        ```
        {
            "name":"example02",
            "version":"0.0.1",
            "dependencies":{
            "underscore":"~1.2.1"
            }
        }
        ```
    3. 运行`npm install`
    可以看在underscore库安装在了node_modules文件夹下

## Node.js目的
Node.js网站提供了的队Node.js的一段简短描述
> Node.js是构建在Chrome的JavaScript运行时之上的一个平台，用于简单构建快速的、可扩展的网络应用程序。Node.js使用事件驱动的、非阻塞的I/O模型，这让其既轻量又高效，是运行于不同发布设备上的数据密集型实时应用程序的完美平台。

## 回调
函数可以被作为参数传递到另一个函数中，然后被调用。
```javascript
var fs = require('fs');

fs.readFile('somefile.txt','utf8',function(err,data){
    if(err)throw err;

> Blockquote

    console.log(data);
});
```
1. fs(filesystem)模块被请求，以便在脚本中使用
2. 将文件系统上的文件路径作为第一个参数提供给`fs.readFile`方法
3. 第二个参数是`utf8`，表示文件编码
4. 被回调函数作为第三个参数提供给fs.readFile方法
5. 回调函数的第一个参数是err，用于保存在读取文件时返回错误
6. 回调函数的第二个参数是data，用于保存读取文件所返回的数据
7. 一旦文件被读取，回调就会被调用
8. 如果err为真，那么就会抛出错误
9. 如果err为假，那么来自文件的数据就可以使用

## 使用curl查看HTTP表头

![](https://github.com/zt1991616/blog/raw/master/Image/14070802.png)