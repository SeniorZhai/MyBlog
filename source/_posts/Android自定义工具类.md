title: Android自定义工具类
date: 2014-04-16 13:19:40
categories: Android
tags: [Android,工具类,自定义]
---
##CommonUtil

- boolean hasSDCard()	判断设备是否存在SD卡
- String getRootFilePath()	返回文件系统的根目录，存在SD卡返回SD卡目录
- boolean checkNetState(Context context)	判断是否存在网络
- void showToask(Context context,String tip)	显示Toast通知
- int getScreenWidth(Context context)\int getScreenHeight(Context context)	返回屏幕的宽\长

##FileHelper

- boolean fileIsExist(String filePath)	判断文件是否存在
- InputStream readFile(String filePath) 获取文件的input流
- boolean createDirectory(String filePath) 创建文件目录
- boolean deleteDirectroy(String filePath) 删除文件目录
- boolean writeFile(String filePath,InputStream inputStream) 写入字符串到某文件
