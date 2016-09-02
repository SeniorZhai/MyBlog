title: AndroidDialog
date: 2014-11-06 13:26:49
categories: Android
tags: [Dialog]
---
<!--more-->
## 提示
![](/img/14110601.png)
创建代码
```java
protected void dialog(){
	AlertDialog.Builder builder = new Builder(mContext);
	builder.setMessage("确定退出么？");
	builder.setTitle("提示");
	builder.setPostiveButton("确认",new OnClickListener(){
		@Override
		public void onClick(DialogInterface dialog,int which) {
			dilog.dismiss();
			//
		}
	});
	builder.setNegativeButton("取消",new OnClickListner(){
		@Override
		public void onClick(DialogInterface dialog,int which){
			dialog.dismiss();
		}
	});
	builder.create().show();
}
```
## 三个按钮
![](/img/14110602.png)
```java
Dialog dialog = new AlertDialog.Builder(this).setIcon(
　　     android.R.drawable.btn_star).setTitle("喜好调查").setMessage(
　　     "你喜欢李连杰的电影吗？").setPositiveButton("很喜欢",
　　     new OnClickListener() {
　　      @Override
　　      public void onClick(DialogInterface dialog, int which) {
　　       Toast.makeText(Main.this, "我很喜欢他的电影。",
　　         Toast.LENGTH_LONG).show();
　　      }
　　     }).setNegativeButton("不喜欢", new OnClickListener() {
　　    @Override
　　    public void onClick(DialogInterface dialog, int which) {
　　     Toast.makeText(Main.this, "我不喜欢他的电影。", Toast.LENGTH_LONG)
　　       .show();
　　    }
　　   }).setNeutralButton("一般", new OnClickListener() {
　　    @Override
　　    public void onClick(DialogInterface dialog, int which) {
　　     Toast.makeText(Main.this, "谈不上喜欢不喜欢。", Toast.LENGTH_LONG)
　　       .show();
　　    }
　　   }).create();
　　   dialog.show();
```
## 简单的自定View
![](/img/14110603.png)
```java
new AlerDialog.Builder(mContext).setTitle("请输入").setIcon(android.R.drawable.ic_dialog_info)
	.setView(new EditText(this)).setPositiveButton("确定",null)
	.setNegativeButton("取消",null).show()
```
## 消息内容一组单选框
![](/img/14110604.png)
```java
new AlerDialog.Builder(mContext).setTitle("单选框")
		.setMultiChoiceItems(
			new String[]{"Item1","Item2"},null,null)
			.setPositiveButton("确定",null)
			.setNegativeButton("取消",null).show();
```
## 消息内容是一组多选框
![](/img/14110605.png)
```java
new AlerDialog.Builder(mContext).setTitle("多选框")
		.setIcon(android.R.drawable.ic_dialog_info)
		.setSingleChoiceItems(new String[]{"Item1","Item2"},0
			new DialogInterface.OnClickListener(){
				public void onClick(DialogInterface dialog,int which){
					dialog.dismiss();
				}
			}).setNegativeButton("取消",null).show();
```
## 信息内容是一组简单列表项
![](/img/14110606.png)
``java
new AlertDialog.Builder(this).setTitle("列表框").setItems(
　　     new String[] { "Item1", "Item2" }, null).setNegativeButton(
　　     "确定", null).show();
```
## 信息内容自定义布局
![](/img/14110607.png)
```xml
<?xml version="1.0" encoding="utf-8"?>
　　<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
　　 android:layout_height="wrap_content" android:layout_width="wrap_content"
　　 android:background="# ffffffff" android:orientation="horizontal"
　　 android:id="@+id/dialog">
　　 <TextView android:layout_height="wrap_content"
　　   android:layout_width="wrap_content"
　　  android:id="@+id/tvname" android:text="姓名：" />
　　 <EditText android:layout_height="wrap_content"
　　  android:layout_width="wrap_content" android:id="@+id/etname" android:minWidth="100dip"/>
</LinearLayout>
```
```java
LayoutInflater inflater = getLayoutInflater();
　　   View layout = inflater.inflate(R.layout.dialog,
　　     (ViewGroup) findViewById(R.id.dialog));
　　   new AlertDialog.Builder(this).setTitle("自定义布局").setView(layout)
　　     .setPositiveButton("确定", null)
　　     .setNegativeButton("取消", null).show();
```