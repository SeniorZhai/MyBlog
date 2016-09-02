title: SizeClass
date: 2014-12-31 15:14:53
categories: iOS
tags: [SizeClass,多屏幕,适配]
---
对于一个使用Size Class的xib文件，布局上宽度和高度都有三种情况：紧凑(Compact)、任意(Any)、正常(Regular)。14
<!--more-->
在设置Size Class的时候页面会有提示，比如宽为Compact高为Any的情况，提示为3.5-inch、4-inch、4.7-inch的竖屏状态
![](/img/14123101.png)

对于iPad
![](/img/14123102.png)
![](/img/14123103.png)

对于iPhone
![](/img/14123104.png)
![](/img/14123105.png)

对于iPhone6 Plus横屏状态
![](/img/14123106.png)

## 使用
选择在wAny、hAny下，设置约束(Constraint)
![](/img/14123107.png)
这样会在全部尺寸在建立一个统一的约束
示例：
创建一个距离各边距10的View(状态栏是20，所以距离顶部30)
![](/img/14123108.png)
实现的效果为
- iPhone4
![](/img/14123109.png)
- iPhone5
![](/img/14123110.png)
- iPhone6
![](/img/14123111.png)
- iPhone6 Plus
![](/img/14123112.png)
- iPad
![](/img/14123113.png)

### 不同尺寸不同约束
如果需要针对不同尺寸编辑不同的约束，可以用两种方法
1. 在顶部选择Size Class然后编辑约束
2. 选中约束，在属性检查器中选择
![](/img/14123114.png)
![](/img/14123115.png)

对于有的约束在某的尺寸不需要，可以指定尺寸，取消选择`Installed`卸载
![](/img/14123116.png)
也可以切换响应尺寸，删除约束

### 不同尺寸不同View
![](/img/14123117.png)
根据不同尺寸显示跟上述一致

### 不同尺寸不同字体
![](/img/14123118.png)