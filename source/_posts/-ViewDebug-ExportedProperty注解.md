title: '@ViewDebug.ExportedProperty注解'
date: 2016-01-12 21:16:27
categories: Android
tags: [注解]
---
使用`@ViewDebug.ExportedProperty`注解，可以在Monitor的Hierarchy Viewer中的调试View属性，可以直接观察注解过的变量和方法的值，实时观察View的状态变化。
<!--more-->
##使用
- category 指定属性的类别，比如`measurement`,`layout`,`drawing`等。
- resolveId 当resolveId为true时，变量或方法的值为int数据，那么这个值会被转换成Android对应的资源名称
- mapping 可以将int值映射成指定的字符串
- indexMapping 可以将数组的序号映射成指定的字符串
```java
@ViewDebug.ExportedProperty(category = "seniorzhai")
int x = 1;

@Override
@ViewDebug.ExportedProperty(category = "seniorzhai")
public boolean isFocused() {
  return true;
}

@Override
@ViewDebug.ExportedProperty(category = "seniorzhai")
public boolean isFocused() {
  return true;
}

@ViewDebug.ExportedProperty(category = "seniorzhai",resolveld = true)
int b = R.id.no;

@Override
@ViewDebug.ExportedProperty(category = "seniorzhai",
    mapping = {
                  @ViewDebug.IntToString(from = VISIBLE, to = "SENIOR_VISIBLE"),
                  @ViewDebug.IntToString(from = INVISIBLE, to = "SENIOR_INVISIBLE"),
                  @ViewDebug.IntToString(from = GONE, to = "SENIOR_GONE")
              })
public int getVisibility() {
  return super.getVisibility();    
}

@ViewDebug.ExportedProperty(category = "seniorzhai",
indexMapping = {
                  @ViewDebug.IntToString(from = 0, to = "SENIOR_FIRST"),            @ViewDebug.IntToString(from = 1, to = "SENIOR_SECOND"),
                  @ViewDebug.IntToString(from = 2, to = "SENIOR_THIRD")
                })
int[] elements = new int[] {123, 223, 323};
```
