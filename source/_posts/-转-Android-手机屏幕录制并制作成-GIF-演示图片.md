title: "[转]Android 手机屏幕录制并制作成 GIF 演示图片"
date: 2015-06-16 16:41:03
categories: Android
tags: [录屏]
---
<!--more-->
## 声明
本教程，也不应该叫做教程，反正就是毫无技术；只能说是纯手工打造，当然需要完成该教程还是需要你动动手指才行滴~~
话说主要是前3天连续写了3天长文章的博客，一章比一章用心，最后一章写到了昨晚1：30才写好；汗~真的是Hold 不住了；今天就来一个轻松一点儿的吧~~
为什么？
自己想分享手机屏幕短片给朋友看看怎么办？
想给你的软件做一个展示GIF怎么办？
不知道还有没有其他原因；我就这么两个了~~
## 开工
这个就来一个3部曲吧，走起！！！
## 录制
前提：手机版本》=4.0；电脑有 adb
现在手机用数据线连接你的电脑，就是两边都用USB插上~~
电脑 CMD 命令输入：`adb shell screenrecord /sdcard/movie.mp4`
![](http://img.blog.csdn.net/20150107233007951?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcWl1anVlcg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
如果显示如上面所示则进入了录制了~~然后动动你的手指在手机上操作操作~~~
在录像过程中，`可以随时按下Ctrl+C快捷键终止录制操作`。

此时进入你的手机文件中，查看SD卡根目录是不是多了一个`movie.mp4`；现在把该文件拷贝到你的电脑上~~
注意：
1. 只能使用adb命令
2. 最长时常180秒
3. 没有同步声音

## 导入PS
前提：PS 软件一枚
打开PS：文件-导入-视频帧到图层
![](http://img.blog.csdn.net/20150107234525981?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcWl1anVlcg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
等待一回儿，具体多久看自个电脑了，然后会弹出选择视频文件，此时选择我们的  movie.mp4 文件
选择后，我们可以微调一下开始时间和结束时间，另外可以调整一下转换后的图片密度，最后确认。
![](http://img.blog.csdn.net/20150107234912041?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcWl1anVlcg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
切换窗口模式：窗口-工作区-动感
![](http://img.blog.csdn.net/20150107235122343?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcWl1anVlcg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
然后自己调整 调整 你的时间速度。
导出GIF
调整好了，现在就保存为GIF吧。
点击：文件-存储为Web所用格式...
选择类型：GIF 类型
![](http://img.blog.csdn.net/20150107235555006?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcWl1anVlcg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
为了让你的文件更小，你可以在进入前调整图片的整体大小；同时可以调整其中的颜色与仿色值。
最后点击 存储 吧~~~~
注意：如果你的视频的像素较高，那么在PS中可能会占用大量内存，这个要有心理准备啊~~
## 成果
![](http://img.blog.csdn.net/20150108113034644)

总的来说教程较为简单，今天早上起来看的时候恰好看见有人问怎么截图的，这个就巧了~~哈哈~~~