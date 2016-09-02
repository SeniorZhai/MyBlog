title: Android WebView与JS交互
date: 2014-06-17 13:27:06
categories: Android
tags: [Android]
---
1. 开启WebView的JS支持
```java
	webView.getSettings().setJavaScriptEnabled(true);
```
2. 对WebView进行JavascriptInterface绑定，JS脚本通过这个借口来调用Java代码
```java
	//	this为借口对象，第二个参数为JS中改参数的别名
	webView.addJavascriptInterface(this,"zoe");
```
3. Java中调用JS
```java
	// 调用JS的test函数并传参"test"
	webView.loadUrl("javascript:test('"+test+"')");
```
4. js中调用Java函数
```javascript
	<div id='b'><a onclick="window.zoe.onClick">b.c</a></div>
```

## 示例
layout
```xml
<?xml version="1.0" encoding="utf-8"?><LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:orientation="vertical" >

    <WebView
        android:id="@+id/webview"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        android:layout_weight="11" />

   
    <Button
        android:id="@+id/button1"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:layout_weight="1"
        android:text="调用js无参函数" />
    <Button
        android:id="@+id/button2"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:layout_weight="1"
        android:text="调用js有参函数" />
	
</LinearLayout>
```

activity
```java
public class MainActivity extends Activity {
	
	private WebView contentView = null;
	
	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);
		contentView = (WebView)findViewById(R.id.webview);
		contentView.getSettings().setJavaScriptEnabled(true);
		contentView.loadUrl("file:///android_asset/web.html");
		contentView.addJavascriptInterface(this, "zoe");
		Button button1 = (Button)findViewById(R.id.button1);
		Button button2 = (Button)findViewById(R.id.button2);
		button1.setOnClickListener(new OnClickListener() {
			@Override
			public void onClick(View v) {
				contentView.loadUrl("javascript:fun1()");
			}
		});
		button2.setOnClickListener(new OnClickListener() {
			@Override
			public void onClick(View v) {
				contentView.loadUrl("javascript:fun2('参数')");
			}
		});
	}
	public void startFunction() {
		Toast.makeText(this, "js调用了java函数", Toast.LENGTH_SHORT).show();
		runOnUiThread(new Runnable() {
			@Override
			public void run() {

			}
		});
	}
	public void startFunction(final String str) {
		Toast.makeText(this, "js调用了java函数，并传参："+str, Toast.LENGTH_SHORT).show();
		runOnUiThread(new Runnable() {
			@Override
			public void run() {

			}
		});
	}
}
```

web.html
```html
<html>
<head>
<meta http-equiv="Content-Type"	content="text/html;charset=utf-8">
<script type="text/javascript">
function fun1(){
	 document.getElementById("content").innerHTML +=   
         "<br\>java调用了js函数fun1";
}

function fun2(arg){
	 document.getElementById("content").innerHTML +=   
         ("<br\>"+"java调用了js函数fun2，并传参："+arg);
}

</script>
</head>
<body>
HTML标题 <br/>
<a onClick="window.zoe.startFunction()">调用java函数</a><br/>
<a onClick="window.zoe.startFunction('hello java')">调用java函数并传递参数</a>
<br/>
<div id="content">这里是内容</div>
</body>
</html>
```
![](https://github.com/zt1991616/blog/raw/master/Image/14061801.png)
[例子](https://github.com/zt1991616/JSDemo/)