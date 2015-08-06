title: 获取NavBar高度的方法
date: 2015-08-05 21:27:04
categories: Android
tags: [NavBar,虚拟键,NavtigationBar]
---
<!--more-->
##判断是否存在虚拟键
```java
private final static String SHOW_NAV_BAR_RES_NAME = "config_showNavigationBar";
public String getNavBarOverride() {
	if (Build.VERSION>SDK_INT >= Build.VERSION_CODES.KITKAT) {
		WindowManager wm = (WindowManager) context.getSystemService(Context.WINDOW_SERVICE);
		try {
			// 相当于SystemProperties.get
			Class c = Class.forName("android.os.SystemProperties");
			Method m = c.getDeclaredMethod("get",String.class);
			m.setAccessible(true);
			return (String)m.invoke(null,"qemu.hw.mainkeys");
		} catch (Throwable e) {
			return null;
		}
	}	
}

@TargetApi(14)
private boolean hasNavBar(Context context) {
	Resources res = context.getResources();
	// resources.getIdenttifier() 方法可以获取指定报名下的资源文件ID，后两个参数表示资源类型和默认报名
	int resourceId = res.getIdentifier(SHOW_NAV_BAR_RES_NAME,"bool","android");
	if(resourceId != 0){
		if ("1".equals(getNavBarOverride())) {
			hasNav = false;
		} else if ("0".equals(getNavBarOverride())) {
			hasNav = true;
		}
		return hasNav;
	} else {
		return !ViewConfiguration.get(context).hasPermanentMenuKey();
	}
}
```
##获取虚拟键的高度
```java
public int getNavigationBarHeight(Context context) {
	Resources res = context.getResources();
	int result = 0;
	if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.ICE_CREAM_SANDWICH) {
		if (hasNavBar(context)) {
			String key;
			if (context.getResources().getConfiguration().orientation == Configuration.ORIENTATION_PORTRAIT) {
			  	key = NAV_BAR_HEIGHT_RES_NAME;
            } else {
                if (!isNavigationAtBottom())
                    return 0;
                key = NAV_BAR_HEIGHT_LANDSCAPE_RES_NAME;
			}
			return getInternalDimensionSize(res,key);
		}
	}
	return result;
}

private int getInternalDimensionSize(Resources res, String key) {
    int result = 0;
    int resourceId = res.getIdentifier(key, "dimen", "android");
    if (resourceId > 0) {
        result = res.getDimensionPixelSize(resourceId);
    }
    return result;
}

private boolean isNavigationAtBottom() {
	WindowManager wm = (WindowManager) context.getSystemService(Context.WINDOW_SERVICE);
	float mSmallestWidthDp = getSmallestWidthDp(wm);
    return (mSmallestWidthDp >= 600 || mInPortrait);
}

private float getSmallestWidthDp(WindowManager wm) {
    DisplayMetrics metrics = new DisplayMetrics();
    if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.JELLY_BEAN) {
        wm.getDefaultDisplay().getRealMetrics(metrics);
    } else {
        //this is not correct, but we don't really care pre-kitkat
        wm.getDefaultDisplay().getMetrics(metrics);
    }  
    float widthDp = metrics.widthPixels / metrics.density;
    float heightDp = metrics.heightPixels / metrics.density;
    return Math.min(widthDp, heightDp);
}
```
