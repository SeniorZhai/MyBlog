title: 非常见Android项目文件
date: 2014-07-17 13:40:14
categories: Android
tags: [Android]
---
## ids.xml
/res/values/ids.xml

`ids.xml`是区别于R文件的一种设置控件ID的方式。使用示例如下：
1. 控件定义时

```xml
<Button 
	android:id = "@id/button_ok"
	...
	/>
```
2. 在ids文件中添加

```xml
<resources>
    <item type="id" name="button_ok">false</item>
    ...
</resources>
```

3.在调用控件时

```java
Button bn = new Button(context);
bn.setId(R.id.button_ok);	// 区别于 bn.setId(context.getResources().getInteger(R.id.button_ok));
```

使用`ids.xml`的优点如下
1. 命名方便，可以先将控件先命名好，在布局时直接命名
2. 使用代码布局时，不需要转换
- 注意：在ids.xml中的每一项也会生成到R文件中

## arrays.xml
用于包装数组
// 在arrays.xml中定义
```xml
<resources>
    <string-array name="week">
        <item>Sunday</item>
        <item>Monday</item>
        <item>Tuesday</item>
        <item>Wednesday</item>
        <item>Thursday</item>
        <item>Friday</item>
        <item>Saturday</item>
    </string-array>
</resource>
```
在Java中调用
```java
CharSequence[] items = this.getResources().getStringArray(R.array.reboot_item);
```

## attrs.xml
`attrs.xml`用于设定自定义属性
1. 在`res/values`文件夹下定义一个`attrs.xml`文件
```xml
<?xml version="1.0" encoding="utf-8" ?>
<resources>
    <declare-styleable name="MyView">
        <attr name="textColor" format="color" />
    </declare-styleable>
</resource>
```
2. 在Java调用自定义属性
```java
public class MyView extends View {
    private Paint mPaint;
    private Context mContext;
    
    public MyView(Context context) {
        super(context);
        mPaint = new Paint();
    }
    
    public MyView(Context context,AttributeteSet atts){
        super(context,attrs);
        mPaint = new Paint();
        TypedArray a = context.obtainStyledAttributes(attrs,R.styleable.MyView);　
        // R.styleable.MyView_textColor是读取attrs中参数名，以"样式名_参数名"的形式
        // 第二个参数为默认值，如果从xml中获取不到则使用默认值
        int textColor = a.getColor(R.styleable.MyView_textColor,0XFFFFFFFF);
        mPaint.setTextColor(textColor);
        a.recycle();
    }
    @Override　　　　
　　protected　void　onDraw(Canvas　canvas)　{　　　　
        　super.onDraw(canvas);　　　　　
　　　　　//设置填充　　　　　
　　　　　mPaint.setStyle(Style.FILL);　　　　　　
　　　　　//画一个矩形,前俩个是矩形左上角坐标，后面俩个是右下角坐标　　　　　
　　　　　canvas.drawRect(new　Rect(10,　10,　100,　100),　mPaint);　　　　　　　　　
　　　　　mPaint.setColor(Color.BLUE);　　　　　
　　　　　//绘制文字　　　　　
　　　　  canvas.drawText(mString,　10,　110,　mPaint);　　　　　　　　    
　　}
}
```
3. 布局时使用属性
```xml
<?xml version="1.0"　encoding="utf-8"?>　　　
<com.android.tutor.MyView　　　
　　　　android:layout_width="fill_parent"　　　　
　　　　android:layout_height="fill_parent"　　　　
　　　　test:textSize="20px"　　
　　　　test:textColor="# fff"　　
/>　　　
```
