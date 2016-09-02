title: NiftyDialogEffects
date: 2014-08-15 13:57:55
categories: Android
tags: [Android]
---
一个拥有丰富动画的对话框

![](https://github.com/zt1991616/blog/raw/master/Image/14081501.png)

![](https://github.com/zt1991616/blog/raw/master/Image/14081502.png)
## 使用
```java
// 获取一个对话框对象
NiftyDialogBuilder dialogBuilder = NiftyDialogBuilder.getInstance(this);
dialogBuilder.withTitle("弹出框")						// 标题
				.withTitleColor("# FFFFFF")				// 标题的颜色
				.withDividerColor("# 11000000")			// 分隔栏的颜色
				.withMessage("这是信息文本。")			// 文本信息
				.withMessageColor("# FFFFFF")			// 文本信息的颜色
				.withIcon(getResources().getDrawable(R.drawable.icon))	// 标题栏的图标
				.isCancelableOnTouchOutside(true) 		// 外部点击退出
				.withDuration(500) 						// 动画延迟
				.withEffect(effect) 					// 动画类别 
				.withButton1Text("确定") 				// 按钮1的文本
				.withButton2Text("取消") 				// 按钮2的文本
				.setCustomView(R.layout.custom_view, v.getContext()) // 设置自定义view
				.setButton1Click(new View.OnClickListener() {
					@Override
					public void onClick(View v) {
						Toast.makeText(v.getContext(), "确定",
								Toast.LENGTH_SHORT).show();
					}
				}).setButton2Click(new View.OnClickListener() {
					@Override
					public void onClick(View v) {
						Toast.makeText(v.getContext(), "取消",
								Toast.LENGTH_SHORT).show();
					}
				}).show();
```
---
## 自定义内容
NiftyDialogEffects的color文件内可以修改对话框的颜色
```xml
	<color name="text_color"># FFFFFF</color>
    <color name="divider_color"># 11000000</color>
    <color name="msg_color"># FFFFFFFF</color>
    <color name="dialog_bg"># FF4FC1E9</color>

    <color name="btn_press_color"># FF4FC1E9</color>
    <color name="btn_unpress_color"># FF3BAFDA</color>
```
===
[NiftyDialogEffects](https://github.com/zt1991616/NiftyDialogEffects/)