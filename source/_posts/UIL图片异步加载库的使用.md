title: UIL图片异步加载库的使用
date: 2014-10-14 11:14:42
categories: Android
tags: [UIL,异步加载]
---
<!--more-->
##使用
作为图片加载库必须设置网络、SD卡权限
```xml
<uses-permission android:name="android.permission.INTERNET" />  
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />  
```
- 设置缓存目录
```java
File cacheDir = StorageUtils.getOwnCacheDirectory(getApplicationContext(),"imageloader/Cache");
// 设置ImageLoaderConfiguration
.discCache(new UnlimitedDiscCache(cacheDir));
```

###配置ImageLoaderConfiguration
```java
ImageLoaderConfiguration config = new ImageLoaderConfiguration
	.Builder(context)
	.memoryCacheExtraOptions(480,800)	// 保存的每个文件的最大长宽
	.discCacheExtraOptions(480,800,CompressFormat.JPEG,75,null)	// 设置详细信息，最好不要设置
	.threadPoolSize(3)	// 线程池加载的数量
	.threadPriority(Thread.NORM_PRIORITY - 2)
	.denyCacheImageMultipleSizesInMemory()
	.memoryCache(new UsingFreqLimitedMemoryCache(2 * 1024 * 1024))	// 缓存，可以自己实现
	.memoryCacheSize(2 * 1024 * 1024)
	.discCacheSize(50 * 1024 * 1024)
	.discCacheFileNameGenerator(new Md5FileNameGenerator())	// 保存URI名称用MD5加密 可以使用HashCodeFileNameGenerator
	.tasksProcessingOrder(QueueProcessingType.LIFO)
	.discCacheFileCount(100)	// 缓存文件数量
	.discCache(new UnlimitedDiscCache(cacheDir))	// 自定义缓存路径
	.defaultDisplayImageOptions(DisplayImageOptions.createSimple())	
	.imageDownloader(new BaseImageDownloader(context,5 * 1000,30 * 1000))	// 连接超时5s，读取超时30s
	.writeDebugLogs()
	.build();	// 开始构建
ImageLoader.getInstance().init(config);	
```

###加载
```java
// 获取ImageLoader实例
ImageLoader imageLoader = ImageLoader.getInstance();
// 设置配置
DisplayImageOptions option;
options = new DisplayImageOptions.Builder()
	.showImageOnLoading(R.drawable.ic_launcher)	// 设置图片在下载期间显示的图片
	.showImageForEmptyUri(R.drawable.ic_launcher)	// 设置图片Uri为空或者是错误的时候显示的图片
	.showImageOnFail(R.drawable.ic_laucher)	// 设置图片加载、解码过程错误的时候显示的图片
	.cacheInMemory(true)	// 设置下载的图片是否缓存在内存中
	.cacheOnDisc(true)	// 设置下载的图片是否缓存在SD卡中
	.considerExifParams(true)	// 是否考虑JPEG图像EXIF参数(旋转、翻转)
	.imageScaleType(ImageScaleType.EXACTLY_STRETCHED)	//设置图片以何种编码方式显示
	.bitmapConfig(Bitmap.Config.RGB_565)	// 设置图片的解码类型
	.decodingOptions(android.graphics.BitmapFactory.Options decodingOptions)	// 设置图片的解码配置
	.delayBeforeLoading(int delayInMillis)	// 下载完的延迟时间
	.preProcessor(BitmapProcessor preProcessor)	// 设置图片加入缓存前，对bitmap进行设置
	.resetViewBeforeLoading(true)	// 设置图片在下载前是否重置，复位
	.displayer(new RondedBitmapDisplayer(20))	// 是否设置为圆角
	.displayer(new FadeInBitmapDisplayer(100))	// 是否显示加载渐入动画
	.build(); // 构建完成
```
> 不需要的可以不做配置

1. .imageScaleType(ImageScaleType imageScaleType)	设置图片的缩放方式
	`EXACTLY`:图像将完全按比例缩小到目标大小
	`EXACTLY_STRETCHED`:图片会缩放到目标大小
	`IN_SAMPLE_INT`:图像将被二次采样的整数倍
	`IN_SAMPLE_POWER_OF_2`:图片将降低2倍，直到下一减少步骤，使图像更小的目标大小
	`NONE`:图片不会调整
2. .displayer(BitmapDisplayer displayer)   是设置 图片的显示方式
	RoundedBitmapDisplayer(int roundPixels) 设置圆角图片
	FakeBitmapDisplayer() 这个类什么都没做
	FadeInBitmapDisplayer(int durationMillis) 设置图片渐显的时间 
	SimpleBitmapDisplayer() 正常显示一张图片　　
```java
ImageLoader.getInstance().displayImage(imageUrl, imageView); // imageUrl代表图片的URL地址，imageView代表承载图片的IMAGEVIEW控件  
ImageLoader.getInstance().displayImage(imageUrl, imageView，options); // imageUrl代表图片的URL地址，imageView代表承载图片的IMAGEVIEW控件 ， options代表DisplayImageOptions配置文件  
```

###加载监听
```java
imageLoader.displayImage(imageUrl, imageView, options, new ImageLoadingListener() {  
    @Override  
    public void onLoadingStarted() {  
       //开始加载的时候执行  
    }  
    @Override  
    public void onLoadingFailed(FailReason failReason) {        
       //加载失败的时候执行  
    }   
    @Override   
    public void onLoadingComplete(Bitmap loadedImage) {  
       //加载成功的时候执行  
    }   
    @Override   
    public void onLoadingCancelled() {  
       //加载取消的时候执行  
  
    }});  
```

###加载监听进度
```java
imageLoader.displayImage(imageUrl, imageView, options, new ImageLoadingListener() {  
    @Override  
    public void onLoadingStarted() {  
       //开始加载的时候执行  
    }  
    @Override  
    public void onLoadingFailed(FailReason failReason) {        
       //加载失败的时候执行  
    }      
    @Override      
    public void onLoadingComplete(Bitmap loadedImage) {  
       //加载成功的时候执行  
    }      
    @Override      
    public void onLoadingCancelled() {  
       //加载取消的时候执行  
    },new ImageLoadingProgressListener() {        
      @Override  
      public void onProgressUpdate(String imageUri, View view, int current,int total) {     
      //在这里更新 ProgressBar的进度信息  
      }  
    });  
```

##注意事项
1. 权限
2. 必须初始化ImageLoader.getInstance().init(config)
3. ImageLoader是根据ImageView的height，width确定图片的宽高
4. 如果经常出现OOM
	- ①减少配置之中线程池的大小，`.(.threadPoolSize)`推荐1-5；
   	- ②使用`.bitmapConfig(Bitmap.config.RGB_565)`代替ARGB_8888;
    - ③使用`.imageScaleType(ImageScaleType.IN_SAMPLE_INT)`或者`try.imageScaleType(ImageScaleType.EXACTLY)`；
   	- ④避免使用`RoundedBitmapDisplayer.`他会创建新的`ARGB_8888`格式的Bitmap对象；
   	- ⑤使用`.memoryCache(new WeakMemoryCache())`，不要使用`.cacheInMemory()`;

##其他
```java
String imageUri = "http://site.com/image.png"; // from Web  
String imageUri = "file:///mnt/sdcard/image.png"; // from SD card  
String imageUri = "content://media/external/audio/albumart/13"; // from content provider  
String imageUri = "assets://image.png"; // from assets  
String imageUri = "drawable://" + R.drawable.image; // from drawables (only images, non-9patch)  
```