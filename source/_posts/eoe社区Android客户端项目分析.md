title: eoe社区Android客户端项目分析
date: 2014-05-07 13:22:50
categories: Android
tags: [Android]
---
## **一、工程目录结构**
src存放java源文件
**src**<br>
>├ cn.eoe.app --存放程序全局性类的包<br>
>├ cn.eoe.app.adapter --存放适配器的实现类的包 <br>
>├ cn.eoe.app.adapter.base --存放适配器基类的包<br>
>├ cn.eoe.app.biz --存放ＤＡＯ类的包<br>
>├ cn.eoe.app.config －－存放常量，配置和api接口等类的包<br>
>├ cn.eoe.app.db --关于sqlite操作相关的类的包<br>
>├ cn.eoe.app.db.biz --详细的增删改查类的包，暂时仅有一个类<br>
>├ cn.eoe.app.entity --实体类包<br>
>├ cn.eoe.app.entity.base --实体类基类包<br>
>├ cn.eoe.app.https --网络访问相关类的包<br>
>├ cn.eoe.app.indicator --导航相关的类包<br>
>├ cn.eoe.app.slidingmenu --滑动菜单相关类包<br>
>├ cn.eoe.app.ui --界面相关的包，activity的类<br>
>├ cn.eoe.app.ui.base --activity相关的基类包<br>
>├ cn.eoe.app.utils --工具类包<br>
>├ cn.eoe.app.view --Fragment相关类的包<br>
>├ cn.eoe.app.widget --自定义view组件包<br>
>
>├ com.google.zxing.camera --第三方定义，控制摄像头包<br>
>├ com.google.zxing.decoding -- 二维码图像解码包<br>
>├ com.google.zxing.view -- 自定义View，控制拍摄取景框和动画等<br>

**libs**<br>
libs目录存放项目引用的第三方jar包
>├  android-support-v4.jar --v4兼容包<br>
>├ jackson-all-1.9.2.jar --解析json的包<br>
>├ umeng_sdk.jar --友盟的sdk<br>
>├ zxing-1.6.jar --二维码处理的包<br>

## 二、Api接口数据分析

http://http://api.eoe.name/client/top
```javascript
{
    "response": {
        "date": 1399446965,
        "categorys": [
            {
                "name": "精选教程",
                "url": "http://api.eoe.name//client/wiki?k=lists&t=top"
            },
            {
                "name": "精选资讯",
                "url": "http://api.eoe.name//client/news?k=lists&t=top"
            },
            {
                "name": "精选博客",
                "url": "http://api.eoe.name//client/blog?k=lists&t=top"
            }
        ],
        "list": [
            {
                "name": "精选教程",
                "more_url": "http://api.eoe.name//client/news?k=lists&pageNum=2&t=top",
                "items": [
                    {
                        "id": "17739",
                        "thumbnail_url": "",
                        "title": "给准备跳槽的互联网从业人员提个醒",
                        "time": "1394029410",
                        "short_content": "　　早春三月，又到了一年一度的跳槽高峰。虽然东楼目...",
                        "detail_url": "http://api.eoe.name/client/news?k=show&id=17739"
                    },
                    ……
                ]
            },
            {
                "more_url": "http://api.eoe.name/client/wiki?k=lists&pageNum=2&t=top",
                "name": "精选资讯",
                "items": [
                    {
                        "title": "创建你的第一个Android项目",
                        "id": "242",
                        "time": "1353410798",
                        "detail_url": "http://api.eoe.name/client/wiki?k=show&id=242"
                    },
                    ……
                ]
            },
            {
                "name": "精选博客",
                "more_url": "http://api.eoe.name//client/blog?k=lists&pageNum=2&t=top",
                "items": [
                    {
                        "id": "3348",
                        "name": "过期的白砂糖",
                        "head_image_url": "http://bbs.eoe.name/uc_server/avatar.php?uid=739935&size=small",
                        "title": "GitHub上最火的40个Android开源项目（一）",
                        "time": "1367809230",
                        "short_content": "GitHub上最火的40个Android开源项目（一）GitHub上最...",
                        "detail_url": "http://api.eoe.name/client/blog?k=show&id=3348"
                    },
                    ……
                ]
            }
        ]
    }
}
```
- date:日期
- categorys:类别（分别是精选教程、精选资讯、精选博客）
- list:文章列表
	+ name:名字
	+ more_url:更多文章URL
	+ items:文章数组

分析下文章数组的数据
- 精选教程item
    + id:文章的id
    + thumbnail_url:缩略图url
    + title:标题
    + short_content:内容梗概
    + detil_url:文章的url
- 精选资讯item
    + title:标题
    + time:发表时间
    + detail_url:文章url
    + id:文章id
- 精选博客
    + id:文章id
    + name:作者名称
    + head_image_url:作者头像
    + title:文章标题
    + time:发表时间
    + short_content:文章梗概
    + detail_url:文章url

http://api.eoe.cn/client/news?k=lists&t=top
资讯推介
````javascript
{
    "response": {
        "name": "推荐资讯",
        "more_url": "http://api.eoe.cn//client/news?k=lists&pageNum=2&t=top",
        "items": [
            {
                "id": "17739",
                "thumbnail_url": "",
                "title": "给准备跳槽的互联网从业人员提个醒",
                "time": "1394029410",
                "short_content": "　　早春三月，又到了一年一度的跳槽高峰。虽然东楼目...",
                "detail_url": "http://api.eoe.cn/client/news?k=show&id=17739"
            },
            ……
        ]
    }
}
```
- 格式基本相同

推介博客
http://api.eoe.cn/client/blog?k=lists&t=top
```javascript
{
    "response": {
        "name": "推荐博客",
        "more_url": "http://api.eoe.cn//client/blog?k=lists&pageNum=2&t=top",
        "items": [
            {
                "id": "3348",
                "name": "过期的白砂糖",
                "head_image_url": "http://www.eoeandroid.com/uc_server/avatar.php?uid=739935&size=small",
                "title": "GitHub上最火的40个Android开源项目（一）",
                "time": "1367809230",
                "short_content": "GitHub上最火的40个Android开源项目（一）GitHub上最...",
                "detail_url": "http://api.eoe.cn/client/blog?k=show&id=3348"
            },
            ……
        ]
    }
}
```

推荐教程
http://api.eoe.cn/client/wiki?k=lists&t=top
```javascript
{
    "response": {
        "more_url": "http://api.eoe.cn/client/wiki?k=lists&pageNum=2&t=top",
        "name": "推荐教程",
        "items": [
            {
                "title": "创建你的第一个Android项目",
                "id": "242",
                "time": "1353410798",
                "detail_url": "http://api.eoe.cn/client/wiki?k=show&id=242"
            },
            ……      
        ]
    }
}
```
