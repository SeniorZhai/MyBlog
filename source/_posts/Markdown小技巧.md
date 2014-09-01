title: Markdown小技巧
date: 2014-08-01 10:46:30
categories: 杂文
tags: [Markdown]
---
Markdown删除线和插入视频和音乐的方法
<!--more-->
##删除线
使用`~~`包括文本，使得文本有删除线的效果
~~这是错误的文本~~
##视频
嵌入视频的方法和音乐类似，视频网站每个视频页面都会有一个『分享』或『转帖』按钮，点击可以查看代码。
```html
<embed src="http://player.youku.com/player.php/sid/XNzQxMjU2ODI0/v.swf" allowFullScreen="true" quality="high" width="480" height="400" align="middle" allowScriptAccess="always" type="application/x-shockwave-flash"></embed>
```

<embed src="http://player.youku.com/player.php/sid/XNzQxMjU2ODI0/v.swf" allowFullScreen="true" quality="high" width="480" height="400" align="middle" allowScriptAccess="always" type="application/x-shockwave-flash"></embed>
##音乐
以『虾米音乐』为例，歌曲页面有个『转帖』选项，将html代码或javascript代码复制到文中即可。
```html
<embed src="http://www.xiami.com/widget/0_1773340641/singlePlayer.swf" type="application/x-shockwave-flash" width="257" height="33" wmode="transparent"></embed>
```

<embed src="http://www.xiami.com/widget/0_1773340641/singlePlayer.swf" type="application/x-shockwave-flash" width="257" height="33" wmode="transparent"></embed>
