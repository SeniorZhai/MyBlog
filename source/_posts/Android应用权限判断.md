title: Android应用权限判断
date: 2015-04-10 17:05:12
categories: Android
tags: [权限]
---
<!--more-->
##判断应用是否有某个权限
```java
PackageManager pm = getPackageManager();
boolean permission = (PackageManager.PERMISSION_GRANTED == pm.checkPermission("android.permission.RECORD_AUIO","packageName"));// permission用于判断是否有该权限
```
##获取某个应用的权限清单
```java
PackageInfo pack = pm.getPackageInfo("packageName",PackageManager.GET_PERMISSIONS);
String[] permissionStrings = pack.requestedPermissions;	// 获取到应用权限的字符串数组
```