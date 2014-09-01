title: Action Bar选项
date: 2014-07-30 13:44:23
categories: Android
tags: [Android]
---
##showAsAction
- always:一直显示在Action Bar上
- isRoom:如果有足够空间，这个值就会显示在Action Bar上
- nerver:永远不会出现在Action Bar上
- withText:起显示文本标题
- collapse:声明了这个操作视窗应该被折叠到一个按钮中，当用户选择这个按钮时，这个操作视窗展开。否则，
这个操作视窗在默认的情况下是可见的，并且即便在用于不适用的时候，也要占据操作栏的有效空间。
一般要配合ifRoom一起使用才会有效果。
##隐藏域显示
```java
actionBar.hide();
actionBar.show();
```
##Action 下拉导航、视窗导航、标签导航
- 下拉导航

![](https://github.com/zt1991616/blog/raw/master/Image/14073001.png)

1. 初始化一个SpinnerAdapter
```java
SpinnerAdapter mSpinnerAdapter = ArrayAdapter.createFromResource(this,R.array.action_list,android.R.layout.simple_spinner_dropdown_item);
```
2. 生成一个OnNavigationListener来响应ActionBar的菜单项点击操作
```java
 OnNavigationListener mOnNavigationListener = new OnNavigationListener() {  
        @Override  
        public boolean onNavigationItemSelected(int position, long itemId) {  
            Fragment newFragment = null;  
            switch (position) {  
            case 0:  
                newFragment = new Fragment1();  
                break;  
            case 1:  
                newFragment = new Fragment2();  
                break;  
            case 2:  
                newFragment = new Fragment3();  
                break;  
            default:  
                break;  
            }  
            getSupportFragmentManager().beginTransaction()  
                    .replace(R.id.container, newFragment, strings[position])  
                    .commit();  
            return true;  
        }  
    };  
```
3. 绑定适配器和监听器到ActionBar
```java
actionBar = getActionBar();  
actionBar.setNavigationMode(ActionBar.NAVIGATION_MODE_LIST);//导航模式必须设为NAVIGATION_MODE_LIST  
actionBar.setListNavigationCallbacks(mSpinnerAdapter,  
        mOnNavigationListener);  
```

- 视窗模式

![](https://github.com/zt1991616/blog/raw/master/Image/14073002.png)

```xml
<?xml version="1.0" encoding="utf-8"?>  
<menu xmlns:android="http://schemas.android.com/apk/res/android" >  
  
    <item  
        android:id="@+id/menu_search"  
        android:actionViewClass="android.widget.SearchView"  
        android:icon="@drawable/ic_menu_search"  
        android:showAsAction="ifroom"  
        android:title="搜索"/>  
    <item  
        android:id="@+id/menu_share"  
        android:actionProviderClass="android.widget.ShareActionProvider"  
        android:showAsAction="never"  
        android:title="分享"/>  
    <item  
        android:id="@+id/menu_setting"  
        android:actionProviderClass="com.example.tabdemo.MyActionProvider"  
        android:showAsAction="never"  
        android:title="设置">  
        <menu>  
            <item  
                android:id="@+id/menu_theme"  
                android:actionProviderClass="com.example.tabdemo.MyActionProvider"  
                android:showAsAction="always|withText"  
                android:title="更换主题"/>  
            <item  
                android:id="@+id/menu_system"  
                android:actionProviderClass="com.example.tabdemo.MyActionProvider"  
                android:showAsAction="always|withText"  
                android:title="系统设置"/>  
        </menu>  
    </item>  
</menu>  
```
- actionProviderClass:指定一个构建视图所使用的布局资源，还可以使用actionLayout或者actionViewClass。

```java
getMenuInflater().inflate(R.menu.options, menu);  
  
//搜索视窗，因为showAsAction="ifRoom"，所以图三中出现了搜索按钮  
SearchView searchView = (SearchView) menu.findItem(R.id.menu_search)  
        .getActionView();  
  
//分享视窗，因为showAsAction="never"，所以只能在溢出菜单中才看见到  
ShareActionProvider mShareActionProvider = (ShareActionProvider) menu  
        .findItem(R.id.menu_share).getActionProvider();  
Intent shareIntent = new Intent(Intent.ACTION_SEND);  
shareIntent.setType("image/*");  
mShareActionProvider.setShareIntent(shareIntent);  
  
//设置视窗，MyActionProvider就是我们自定义的ActionProvider  
MyActionProvider myactionprovider = (MyActionProvider) menu.findItem(  
        R.id.menu_setting).getActionProvider();  
return super.onCreateOptionsMenu(menu);  
```
- 实现自定义MyActionProvider
```java
public class MyActionProvider extends ActionProvider{  
  
    private Context context;  
    private LayoutInflater inflater;  
    private View view;  
    private ImageView button;  
    public MyActionProvider(Context context) {  
        super(context);  
        // TODO Auto-generated constructor stub  
        this.context = context;  
        inflater = LayoutInflater.from(context);  
        view = inflater.inflate(R.layout.myactionprovider, null);  
    }  
  
      
    @Override  
    public View onCreateActionView() {  
        // TODO Auto-generated method stub  
        button = (ImageView) view.findViewById(R.id.button);  
        button.setOnClickListener(new View.OnClickListener() {  
              
            @Override  
            public void onClick(View v) {  
                // TODO Auto-generated method stub  
                Toast.makeText(context, "是我，没错", Toast.LENGTH_SHORT).show();  
            }  
        });  
        return view;  
    }  
}
```
