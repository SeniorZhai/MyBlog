title: AliasActivity
date: 2014-04-10 13:18:13
categories: Android
tags: [Android]
---
存根Activity，用这个Activity来加载其他的Activity，为了重复使用Activity使用，它的子类必须实现onCreate()方法。可以在onCreate()方法中调用finish()方法，这时Activity跳过生命周期直接调用onDestroy()方法。
activity-alias具体属性有：
- `android:targetActivity` 目标Activity，这个属性的值必须是声明
- `android:name` alias的唯一标识
- `android:enabled` 是否运行aliasActivity加载targetActivity，缺省为true
- `android:exported` 是否运行其他的Application通过使用aliasActivity来加载targetActivity

```XML
    <activity
            android:name="com.example.aliasactivitydemo.MainActivity"
            android:label="@string/app_name" >
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    <activity-alias 
            android:name="AndroidAlias"
            android:targetActivity="MainActivity"
            android:label="Alias"
            android:icon="@drawable/ic_launcher">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />  
            	<category android:name="android.intent.category.DEFAULT" />  
            	<category android:name="android.intent.category.LAUNCHER" /> 
            </intent-filter>
    </activity-alias> 
```
[示例](https://github.com/zt1991616/AliasActivityDemo)

![](https://github.com/zt1991616/blog/raw/master/Image/14041001.png)
![](https://github.com/zt1991616/blog/raw/master/Image/14041002.png)
![](https://github.com/zt1991616/blog/raw/master/Image/14041003.png)

# ExpendableListActivity
一个具有可展开list，其中的item通过ExpandableListAdapter接口来绑定数据源。当用户选择其中某一项时可以自己去定义处理方法。ExpendableListActivity含有一个ExpandableView对象，用两层的方法来展示数据，第一层是组，第二层是子项。使用自定义的xml来制定布局，则ExpandableListView一定要用"@id/android:list"作为id，另外使用一个id@“@id/android/empty”来表示空的list.

main.xml
```xml
	<ExpandableListView 
	    android:id="@id/android:list"
	    android:layout_width="fill_parent"
	    android:layout_height="fill_parent"
	    android:layout_weight="1"
	    android:drawSelectorOnTop="false"
	    />
	<TextView 
	    android:id="@id/android:empty"
	    android:layout_width="fill_parent"
	    android:layout_height="fill_parent"
	    android:text="No data"/>
```
child.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical" >
    <TextView android:id="@+id/child"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        android:padding="50px"
        android:paddingTop="5px"
        android:paddingBottom="5px"
        android:textSize="20px"
        android:text="NO data"/>

</LinearLayout>
```
group.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical" >
    <TextView android:id="@+id/group"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        android:padding="60px"
        android:paddingTop="10px"
        android:paddingBottom="10px"
        android:textSize="26px"
        android:text="No data"
        />
</LinearLayout>
```
MainActivity.java
```java
	List<Map<String,String>> groups = new ArrayList<Map<String,String>>();
		Map<String, String> group1 = new HashMap<String, String>();
		Map<String, String> group2 = new HashMap<String, String>();
		group1.put("group", "DOTA");
		group2.put("group","DiaBlo");
		groups.add(group1);
		groups.add(group2);
		//
		List<Map<String, String>> child1 = new ArrayList<Map<String,String>>();
		Map<String, String> childData1 = new HashMap<String, String>();
		Map<String, String> childData2 = new HashMap<String, String>();
		childData1.put("child", "Dota1");
		childData2.put("child", "Dota2");
		child1.add(childData1);
		child1.add(childData2);
		//
		List<Map<String, String>> child2 = new ArrayList<Map<String,String>>();
		Map<String, String> child2Data1 = new HashMap<String, String>();
		Map<String, String> child2Data2 = new HashMap<String, String>();
		Map<String, String> child2Data3 = new HashMap<String, String>();
		child2Data1.put("child", "DiaBlo1");
		child2Data2.put("child", "DiaBlo2");
		child2Data3.put("child", "DiaBlo3");
		child2.add(child2Data1);
		child2.add(child2Data2);
		child2.add(child2Data3);
		//
		List<List<Map<String,String>>> childs = new ArrayList<List<Map<String,String>>>();
		childs.add(child1);
		childs.add(child2);
		SimpleExpandableListAdapter adapter = 
				new SimpleExpandableListAdapter(
						this, 					//context
						groups,					//一级目录数据
						R.layout.group, 		//一级目录样式布局文件
						new String[]{"group"},	//指定一级目录数据的key
						new int[]{R.id.group},	//指定一级目录数据显示控件的id
						childs, 				//二级目录的数据
						R.layout.child, 		//二级目录样式布局文件
						new String[]{"child"},	//指定二级目录数据的
						new int[]{R.id.child}	//指定二级目录数据显示控件的
				);
		setListAdapter(adapter);
```
[示例](https://github.com/zt1991616/AliasActivityDemo)

![](https://github.com/zt1991616/blog/raw/master/Image/14041004.png)
![](https://github.com/zt1991616/blog/raw/master/Image/14041005.png)
