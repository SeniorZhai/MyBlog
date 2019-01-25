title: electron初识
date: 2018-07-24 11:46:16
categories: electron
tags: [node,js,electron,desktop]
---

<!--more-->
## 从quickstart开始
```
git clone https://github.com/electron/electron-quick-start
cd electron-quick-start
npm install
npm start
```
可以看到一个简单的electron桌面程序启动了

Electron中分为两类进程，一类是主进程main，另一类是渲染器进程renderer
主进程只有一个，负责对整个应用的管理，包括后台操作，创建GUI，处理GUI和后台交互操作
主进程无法显示窗口，在主进程中调用BrowserWindow模块才能使用不同的窗口，每个窗口调用格子的渲染器进程来讲页面渲染到窗口中
由于在网页里管理原生GUI资源是非常危险而且容易造成资源泄露，所以在网页调用GUI相关API是不允许的，如果要在网页中使用GUI操作，其相对应的渲染进程必须和主进程进行通讯，请求主进程进行相关GUI操作
### 工程目录
工程目录如下
```
- index.html
- main.js
- renderer.js
- package.json
```
`package.json`声明了启动脚本，和进程的入口
```
{
    // ...
    "main": "main.js",
    "scripts": {
        "start": "electron ."
    },
    // ...
}
```
如果没有声明main，Electron会优先加载`index.js`

`main.js`创建窗口和处理系统时间
```js
// APP生命周期模块 原生浏览器窗口模块
const {app, BrowserWindow} = require('electron')
// 保持一个全局的window引用，当JS被GC window不会自动关闭
let mainWindow
// 创建窗口
function createWindow () {
  // 创建浏览器窗口
  mainWindow = new BrowserWindow({width: 800, height: 600})
  // 加载应用的index.html
  mainWindow.loadFile('index.html')
  // 打开开发工具
  // mainWindow.openDevTools()
  mainWindow.on('closed', function () {
    // window关闭时取消引用
    mainWindow = null
  })
}
// Electron完成初始化准备创建窗口时调用
app.on('ready', createWindow)

// 当窗口被关闭，退出
app.on('window-all-closed', function () {    
  // 判断OS X上，关闭时退出程序
  if (process.platform !== 'darwin') {
    app.quit()
  }
})

app.on('activate', function () {
  if (mainWindow === null) {
    createWindow()
  }
})
```