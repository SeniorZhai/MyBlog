title: Palette取色
date: 2015-01-06 10:35:47
categories: Android
tags: [Api,取色]
---
Palette可以从图像中提取突出的颜色，可以利用它把色值赋给ActionBar或者其他空间，可以让界面整个色调统一
<!--more-->
![](/img/15010601.png)
```java
Bitmap bm = BitmapFactory.decodeResouce(getResources(),item.image);
Palette palette = Palette.generate(bm);
if(palette.getLightVibrantColor() != null) {
	getSupportActionBar().setBackgroundDrawable(new ColorDrawable(palette.getLightVibrantColor().getRgb()));  
}
```
Palette可以取出以下突出的颜色
- Vibrant  （有活力）
- Vibrant dark（有活力 暗色）
- Vibrant light（有活力 亮色）
- Muted  （柔和）
- Muted dark（柔和 暗色）
- Muted light（柔和 亮色）

Gradle中的dependencies中添加`compile 'com.android.support:palette-v7:21.0.+'`
[示例](https://github.com/SeniorZhai/PaletteDemo)
![](/img/15010602.png)