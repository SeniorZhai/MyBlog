title: "RelativeLayout常见属性介绍"
date: 2014-07-17 13:41:25
categories: Android
tags: [Android]
---
- 第一类:属性值为`true`或`false`

| 属性   |  说明   |
| :-------- | :------: |
| android:layout_centerHrizontal|水平居中|
| android:layout_centerVertical|垂直居中|
| android:layout_centerInparent|相对于父元素完全居中|
| android:layout_alignParentBottom|贴紧父元素的下边缘|
| android:layout_alignParentLeft|贴紧父元素的左边缘|
| android:layout_alignParentRight|贴紧父元素的右边缘|
| android:layout_alignParentTop|贴紧父元素的上边缘|
| android:layout_alignWithParentIfMissing|如果对应的兄弟元素找不到的话就以父元素做参照物|

- 第二类：属性值必须为id的引用名`“@id/id-name”`

| 属性   |  说明   |
| :-------- | :------: |
| android:layout_below|在某元素的下方|
| android:layout_above|在某元素的的上方|
| android:layout_toLeftOf|在某元素的左边|
| android:layout_toRightOf|在某元素的右边|
| android:layout_alignTop|本元素的上边缘和某元素的的上边缘对齐|
| android:layout_alignLeft|本元素的左边缘和某元素的的左边缘对齐|
| android:layout_alignBottom|本元素的下边缘和某元素的的下边缘对齐|
| android:layout_alignRight|本元素的右边缘和某元素的的右边缘对齐|

- 第三类：属性值为具体的像素值，如30dip，40px

| 属性   |  说明   |
| :-------- | :------: |
| android:layout_marginBottom|离某元素底边缘的距离|
| android:layout_marginLeft |离某元素左边缘的距离|
| android:layout_marginRight |离某元素右边缘的距离|
| android:layout_marginTop |离某元素上边缘的距离|
