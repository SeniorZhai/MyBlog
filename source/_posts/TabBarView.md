title: TabBarView
date: 2014-08-14 13:57:03
categories: Android
tags: [Android]
---
![](https://github.com/zt1991616/blog/raw/master/Image/14081401.gif)
---

##使用
```java
LayoutInflater inflator = (LayoutInflater) this.getSystemService(Context.LAYOUT_INFLATER_SERVICE);
View v = inflator.inflate(R.layout.custom_ab,null);
tabBarView = (TabBarView) v.findViewById(R.id.tab_bar);

getActionBar().setDisplayOptions(ActionBar.DISPLAY_SHOW_SUSTOM);
getActionBar().setCustomView(v);

mSectionsPagerAdapter = new SectionsPagerAdapter(getFramentManager());

mViewPager = (ViewPager) findViewById(R.id.pager);
tabBarView.setViewPager(mViewPager);
```
实现一个adapter
```java
public class SectionsPagerAdapter extends FragmentPagerAdapter implements IconTabProvider {
	private intp[] tab_icons = {R.drawable.ic_tab1,R.drawable.ic_tab2,R.drawable.ic_tab3};

	public SectionsPagerAdapter(FragmentManager fm){
		super(fm);
	}

	@Override
	public Fragment getItem(int position) {
		return PlaceholderFragment.newInstance(position + 1);
	}

	@Override
	public int getCount() {
		return tab_icons.length;
	}

	@Override
	public CharSequence getPageTitle(int position) {
		Locale l = Locale.getDefault();
           	switch (position) {
           	case 0:
                return getString(R.string.title_section1).toUpperCase(l);
            case 1:
                return getString(R.string.title_section2).toUpperCase(l);
            case 2:
                return getString(R.string.title_section3).toUpperCase(l);
            }
            return null;
	}
}
```
===
[TabBarView](https://github.com/zt1991616/TabBarView/tree/master)