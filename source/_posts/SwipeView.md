title: SwipeView
date: 2014-08-20 14:06:59
categories: Android
tags: [Android]
---
<!--more-->
SwipeView提供了同级屏幕中的横向导航。
适配器
```java
public class DemoCollectionPagerAdapter extends FragmentStatePagerAdapter{
	public DemoCollectionPagerAdapter(FragmentManager fm) {
        super(fm);
    }
	@Override
    public Fragment getItem(int i) {
        Fragment fragment = new DemoObjectFragment();
        Bundle args = new Bundle();
        // 我们的对象只是一个整数 :-P
        args.putInt(DemoObjectFragment.ARG_OBJECT, i + 1);
        fragment.setArguments(args);
        return fragment;
    }
	@Override
    public int getCount() {
        return 100;
    }

    @Override
    public CharSequence getPageTitle(int position) {
        return "OBJECT " + (position + 1);
    }
}
```
布局文件
```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.view.ViewPager xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent" 
    android:layout_height="match_parent"
    android:id="@+id/pager">
    <android.support.v4.view.PagerTabStrip
        android:id="@+id/pager_title_strip"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_gravity="top"
        android:background="# 33b5e5"
        android:textColor="# fff"
        android:paddingTop="4dp"
        android:paddingBottom="4dp" />
</android.support.v4.view.ViewPager>
```
Fragment
```java
public class DemoObjectFragment extends Fragment {
	public static final String ARG_OBJECT = "object";

	@Override
	public View onCreateView(LayoutInflater inflater, ViewGroup container,
			Bundle savedInstanceState) {
		// 最后两个参数保证LayoutParam能被正确填充
		View rootView = inflater.inflate(R.layout.fragment_collection_object,
				container, false);
		Bundle args = getArguments();
		((TextView) rootView.findViewById(R.id.textView1)).setText(Integer
				.toString(args.getInt(ARG_OBJECT)));
		return rootView;
	}
}
```

![](https://github.com/zt1991616/blog/raw/master/Image/14082001.png)
