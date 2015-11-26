title: ReactNative-真机运行
date: 2015-11-24 20:07:21
categories: React Native
tags: [图片,Image,Android,iOS]
---
<!--more-->
##网络图片
```js
<Image source={{uri:"https://...."}} style={{width:400,height:400}} />
```
使用网络图片需要手动指定大小

##静态图片
APP使用静态图片，只需要把图片文件放在代码文件夹的某处
```js
<Image source={require('./my-icon.png')} />
```
使用`@2x`,`@3x`文件后缀来区别不同的屏幕精度
```
├── button.js
└── img
    ├── check@2x.png
    └── check@3x.png
```
在`button.js`中这么使用
```js
<Image source={require('./img/check.png')} />
```
1. iOS和Android一致的文件系统
2. 图片和JS代码处在相同的文件夹，组件不需要单独设置
3. 不需要全局命名，不用担心名字的冲突的问题
4. 在实际使用到才会被打包到APP中
5. 增加和修改图片不需要重新编译
6. APP可以得知图片的大小，不需要代码再声明尺寸
7. 通过npm分发组件可以包含图片
> 注意：为了资源机制正常，require中的图片名必须是一个静态的字符串

##使用混合APP的图片资源
如果编写一个混合APP(一部分使用React Native，一部分使用原生代码)
使用APP中的图片资源
```js
<Image source={{uri:'app_icon'}} style={{width:40,height:40}} />
```
> 注意：React Native没用做安全检查，必须保证图片的存在
