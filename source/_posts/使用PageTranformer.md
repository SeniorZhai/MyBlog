title: 使用PageTranformer
date: 2014-12-29 11:14:01
categories: Android
tags: [动画,PagerTranformer]
---
ViewPager自带一个`setPageTransformer`用于设置切换动画，该方法在Api 11，因为其动画使用的是属性动画，所以可以使用`nineoldandroids`来支持动画效果
<!--more-->
Google官方提供了两个PageTransformer示例
- DepthPageTransformer
```java
public class DepthPagerTransformer implements ViewPager.PageTransformer {
	private static final float MIN_SCALE = 0.75f

	public void transformPage(View view,float position) {
		int pageWidth = view.getWidth();
		if(position < -1) {
			view.setAlpha(0);
		} else if(position <= 0) {
			view.Alpha(1);
			view.setTranslationX(0);
			view.setScaleX(1);
			view.setScaleY(1);
		} else if(position <= 1) {
			view.setAlpha(1 - position);
			view.setTranslationX(pageWidth * -position);
			float scaleFactor = MIN_SCALE + (1 - MIN_SCALE) * (1 - Math.abs(position));
			view.setScaleX(scaleFactor);
			view.setScaleY(scaleFactor);
		} else {
			view.setAlpha(0);
		}
	}
}
```
![](/img/14122901.gif)
- ZoomOutPageTransformer
```java
public class ZoomOutPageTransformer implements ViewPager.PageTransformer
{
	private static final float MIN_SCALE = 0.85f;
	private static final float MIN_ALPHA = 0.5f;

	public void transformPage(View view,float position)
	{
		int pageWidth = view.getWidth();
		int pageHeight = view.getHeight();

		if (position < -1) {
			view.setAlpha(0);
		} else if (position <= 1) {
			// 从a页向b页滑动时，a页从0.0->-1，b页从1->0
			float scaleFactor = Math.max(MIN_SCALE, 1 - Math.abs(position));  
            float vertMargin = pageHeight * (1 - scaleFactor) / 2;  
            float horzMargin = pageWidth * (1 - scaleFactor) / 2;  
            if (position < 0)  
            {  
                view.setTranslationX(horzMargin - vertMargin / 2);  
            } else  
            {  
                view.setTranslationX(-horzMargin + vertMargin / 2);  
            }  
  
            view.setScaleX(scaleFactor);  
            view.setScaleY(scaleFactor);  
  
            view.setAlpha(MIN_ALPHA + (scaleFactor - MIN_SCALE)  
                    / (1 - MIN_SCALE) * (1 - MIN_ALPHA));  
		} else {
			view.setAlpha(0);
		}
	}
}
```
![](/img/14122902.gif)
