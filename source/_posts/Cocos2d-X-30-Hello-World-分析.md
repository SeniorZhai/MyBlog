title: Cocos2d-X 3.0 Hello World 分析
date: 2014-06-23 11:35:39
categories: Cocos2d-X
tags: [Cocos2d-X,Hello World]
---
## AppDelegate.h与AppDelegate.cpp文件
这两个文件时Cocos2d-x游戏的入口文件，控制着游戏的生命周期,除去构造函数和析构函数外，共有 3 个方法。

```cpp
// 应用程序启动后将调用这个方法。默认的实现中已经包含了游戏启动后的必要准备
bool applicationDidFinishLaunching()
{
    //  初始化游戏引擎控制器Director,以便启动游戏引擎
    auto director = Director::getInstance();
    auto glview = director->getOpenGLView();
    if(!glview){
        glview = GLView::create("My Game");
        director->setOpenGLView(glview);
    }
    //  启动FPS显示
    director->setDisplayStats(true);
    //  设置绘制间隔
    director->setAnimationInterval(1.0 / 6.0);
    
    auto scence = HelloWorld::createScene();
    director->runWithScene(scence);
    
    return true;
}
```
这段代码首先对引擎进行必要的初始化，然后开启了 FPS 显示。FPS 即每秒帧速率，也就是屏幕每秒重绘的次数。启用了FPS 显示后，当前 FPS 会在游戏的左下角显示。通常在游戏开发阶段，我们会启用 FPS 显示，这样就可以方便地确定游戏运行是否流畅。

接下来是设置绘制间隔。绘制间隔指的是两次绘制的时间间隔，因此绘制间隔的倒数就是 FPS 上限。对于移动设备来说，我们通常都会将 FPS 限制在一个适当的范围内。过低的每秒重绘次数会使动画显示出卡顿的现象，而提高每秒重绘次数会导致设备运算量大幅增加，造成更高的能耗。人眼的刷新频率约为 60 次每秒，因此把 FPS 限定在 60 是一个较为合理的设置，Cocos2d-x 就把绘制间隔设置为 1/60 秒。至此，我们已经完成了引擎的初始化，接下来我们将启动引擎。

最后也是最关键的步骤，那就是创建 Hello World 场景，然后指派 Director 运行这个场景。对于游戏开发者而言，我们需要在此处来对我们的游戏进行其他必要的初始化，例如读取游戏设置、初始化随机数列表等。程序的最末端返回 true，表示程序已经正常初始化。

`void applicationDidEnterBackground()`。当应用程序将要进入后台时，会调用这个方法。具体来说，当用户把程序切换到后台，或手机接到电话或短信后程序被系统切换到后台时，会调用这个方法。此时，应该暂停游戏中正在播放的音乐或音效。动作激烈的游戏通常也应该在此时进行暂停操作，以便玩家暂时离开游戏时不会遭受重大损失。

`void applicationWillEnterForeground()`。该方法与 applicationDidEnterBackground()成对出现，在应用程序回到前台 时被调用。相对地，我们通常在这里继续播放刚才暂停的音乐，显示游戏暂停菜单等。