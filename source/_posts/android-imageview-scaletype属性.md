title: android imageview scaletype属性
date: 2014-08-21 14:07:47
categories: Android
tags: [Android]
---
<!--more-->
scaletype决定了图片在View上显示的方式，可以通过xml中定义`android:scale=`或者代码中调用`imageView.setScaleType()`

示例的原图
128*128

![](https://github.com/zt1991616/blog/raw/master/Image/14082111.gif)
640*428

![](https://github.com/zt1991616/blog/raw/master/Image/14082112.gif)

1. CENTER
按图片的原来size居中显示，当图片长/宽超过View的长/宽，则截取图片的居中部分显示
![](https://github.com/zt1991616/blog/raw/master/Image/14082101.gif)
![](https://github.com/zt1991616/blog/raw/master/Image/14082102.gif)
2. CENTER_CROP
按比例扩大图片的size居中显示，使得图片长(宽)等于或大于view的长(宽)
![](https://github.com/zt1991616/blog/raw/master/Image/14082103.gif)
![](https://github.com/zt1991616/blog/raw/master/Image/14082104.gif)
3. CENTER_INSIDE
将图片的内容完整居中显示，通过按比例缩小或原来的size使得图片长/宽等于或小于View的长/宽 
![](https://github.com/zt1991616/blog/raw/master/Image/14082105.gif)
![](https://github.com/zt1991616/blog/raw/master/Image/14082106.gif)
4. FIT_CENTER
把图片按比例扩大/缩小到View的宽度，居中显示
![](https://github.com/zt1991616/blog/raw/master/Image/14082107.gif)
![](https://github.com/zt1991616/blog/raw/master/Image/14082108.gif)
5. FIT_START, FIT_END
图片缩放效果上与FIT_CENTER一样，只是显示的位置不同，FIT_START是置于顶部，FIT_CENTER居中，FIT_END置于底部。
6. FIT_XY
不按比例缩放图片，目标是把图片塞满整个View。
![](https://github.com/zt1991616/blog/raw/master/Image/14082109.gif)
![](https://github.com/zt1991616/blog/raw/master/Image/14082110.gif)