title: Bootstrap风格按钮
date: 2014-04-21 13:06:36
categories: iOS
tags: [iOS]
---
### 如何使用
- 将`UIButton+Bootstrap`,`NSString+FontAwesome`,`FontAwesome.ttf`文件添加进工程
- 导入`UIButton+Bootstrap.h`
- 在`Info.plist`中的`Fonts provided by application`中包含`FontAwesome.ttf`

创建自定义的UIButoon(把type属性设置为Custom)
设置风格
```objectivec
[yourButton primaryStyle];
[yourOtherButton successStyle];
```
设置图标
```objectivec
[yourButton addAwesomeIcon:FAIconBookmark beforeTitle:YES];
[yourOtherButton addAwesomeIcon:FAIconCalendar beforeTitle:NO];
```
