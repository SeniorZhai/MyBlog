title: PagerAdapter与ViewPager
date: 2014-12-15 10:49:36
categories: Android
tags: [Adapter,PagerAdapter]
---
###PagerAdapter
PagerAdapter是`android.support.v4`包中的类，它的子类有`FragmentPagerAdapter`，`FragmentStatePagerAdapter`，这两个Adapter都是Fragment的适配器，用于实现Fragment的滑动效果。
<!--more-->
PagerAdapter主要是ViewPager的适配器，ViewPager也是`android.support.v4`扩展包中添加的控件，可以实现控件的活动效果。
1. instantiateItem(ViewGroup,int)	// 将显示的控件进行初始化，并加入到ViewGroup中去
2. destroyItem(ViewGroup,int,Object)	// 销毁控件
3. getCount()	// 控件的数量
4. isViewFromObject(View,Object)	// 判断是否是同一对象

###FragmentPagerAdapter
FragemntPagerAdapter用于处理Fragment页面的横向滑动，每一个页面都是一个Fragment，并且每一个Fragment都将会保存到Fragment Manager当中，当用户没有可能再次回到页面时，Fragment Manager才会将其销毁
适用于静态Fragment，每个页面对应的Fragment用户可以访问时会一直在内存中，当页面数量比较大时，建议使用`FragmentStatePagerAdapter`
FragmentPagerAdapter需要实现`getItem(int)`和`getCount()`方法
```java
public class MyAdapter extends FragmentActivity {
	static final int NUM_ITEMS = 10;

	public MyAdapter(FragmentManager fm) {
		super(fm);
	}

	@Override
	public getCount(){
		return NUM_ITEMS;
	}

	@Override
	public Fragment getItem(int position){
		return fragments.get(position);	// 返回Fragment
	}
}
```

###FragmentStatePagerAdapter
FragmentStatePagerAdapter只保留当前页面，当页面离开实现后，就会被消除，释放其资源。
- getItem() 返回Fragment，生成新的Fragment对象，可以使用`setArguments()`传递初始化参数
- instantiateItem() 除非FragmentManager可以从SavedState中恢复对应的Fragment的情况外，该函数会调用`getItem()`函数，生成新的Fragment对象，新的对象将被`FragmentTransaction.add()`
- destroyItem() 将Fragment移除，即调用`FragmentTransaction.remove()`