title: Android-Universal-Image-Loader
date: 2014-05-05 13:21:03
categories: Android
tags: [Android]
---
## 介绍
Android-Universal-Image-Loader是一个开源的图片异步加载库，该项目的目的是提供一个可重复使用的图像异步加载、缓存和显示的工具。该库非常强大，国内外有很多有名的App都在使用。

## 特点
- 多线程的图像加载
- 尽可能多的配置选项（线程池，加载器，解析器，内存/磁盘缓存，显示参数等等）
- 可以添加图片加载监听器
- 可以自定义显示每一张图片时都带不同参数
- 支持Widget
- Android 1.5以上支持

## 使用方法
1.Android Manifest
```xml
<manifest>
	<uses-permission android:name="android.permission.INTERNET" />
	<user-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
	<application android:name="MyApplication">
	...
	</application>
</manifest>
```
2.Application class
```java
public class MyApplication extends Application{
	@Override
	public void onCreate() {
		super.onCreate;
		ImageLoaderConfiguration config = new ImageLoaderConfiguration.Builder(getApplicationContext())
		...
		.build();
		ImageLoader.getInstance().init(config);
	}	
}
```
Configuration
所有的选项都是可选的，只选择你真正制定的去配置。
```java
File cacheDir = StrongeUtils.getCacheDiretory(context);
ImageLoaderConfiguration config = new ImageLoaderConfiguration.Bulider(context).memoryCacheExtraOptions(480,800) //如果图片尺寸大于了这个参数，那么就会按照这个参数对图片大小进行限制并缓存，默认是屏幕大小
.discCacheExtraOptions(480, 800, CompressFormat.JPEG, 75, null)
.taskExecutor(...)
.taskExecutorForCachedImages(...)
.threadPoolSize(3) // default
.threadPriority(Thread.NORM_PRIORITY - 1) // default
.tasksProcessingOrder(QueueProcessingType.FIFO) // default
.denyCacheImageMultipleSizesInMemory()
.memoryCache(new LruMemoryCache(2 * 1024 * 1024))
.memoryCacheSize(2 * 1024 * 1024)
.memoryCacheSizePercentage(13) // default
.discCache(new UnlimitedDiscCache(cacheDir)) // default
.discCacheSize(50 * 1024 * 1024)
.discCacheFileCount(100)
.discCacheFileNameGenerator(new HashCodeFileNameGenerator()) // default
.imageDownloader(new BaseImageDownloader(context)) // default
.imageDecoder(new BaseImageDecoder()) // default
.defaultDisplayImageOptions(DisplayImageOptions.createSimple()) // default
.writeDebugLogs()
.build();
```
Display Options
显示参数可以分别被每一个显示任务调用(ImageLoader.displayImage(…))
```java
DisplayImageOptions options = new DisplayImageOptions.Builder()
        .showImageOnLoading(R.drawable.ic_stub) // 在显示真正的图片前，会加载这些资源
        .showImageForEmptyUri(R.drawable.ic_empty) // 空的Url
        .showImageOnFail(R.drawable.ic_error) // 错误时加载
        .resetViewBeforeLoading(false)  // default
        .delayBeforeLoading(1000) // 延时1000ms加载图片
        .cacheInMemory(false) // default
        .cacheOnDisc(false) // default
        .preProcessor(...)
        .postProcessor(...)
        .extraForDownloader(...)
        .considerExifParams(false) // default
        .imageScaleType(ImageScaleType.IN_SAMPLE_POWER_OF_2) // default
        .bitmapConfig(Bitmap.Config.ARGB_8888) // default
        .decodingOptions(...)
        .displayer(new SimpleBitmapDisplayer()) // default
        .handler(new Handler()) // default
        .build();
```
可接受的URL
```
String imageUri = "http://site.com/image.png"; // from Web
String imageUri = "file:///mnt/sdcard/image.png"; // from SD card
String imageUri = "content://media/external/audio/albumart/13"; // from content provider
String imageUri = "assets://image.png"; // from assets
String imageUri = "drawable://" + R.drawable.image; // from drawables (only images, non-9patch)
```
完整版
```
// Load image, decode it to Bitmap and display Bitmap in ImageView
imageLoader.displayImage(imageUri, imageView, displayOptions, 
new ImageLoadingListener() {
    @Override
    public void onLoadingStarted(String imageUri, View view) {
        ...
    }
    @Override
    public void onLoadingFailed(String imageUri, View view, FailReason failReason) {
        ...
    }
    @Override
    public void onLoadingComplete(String imageUri, View view, Bitmap loadedImage) {
        ...
    }    
    @Override
    public void onLoadingCancelled(String imageUri, View view) {
        ...
    }
},new ImageLoadingProgressListener(){
	 @Override
    public void onProgressUpdate(String imageUri, View view, int current, int total) {
        ...
    }
});


ImageSize targetSize = new ImageSize(120, 80); // result Bitmap will be fit to this size
imageLoader.loadImage(imageUri, targetSize, displayOptions, new     SimpleImageLoadingListener() {
    @Override
    public void onLoadingComplete(String imageUri, View view, Bitmap loadedImage) {
        // Do whatever you want with Bitmap
    }
});
// 同步解码Bitmap
Bitmap bmp = imageLoader.loadImageSync(imageUri, targetSize, displayOptions);
```
## 用户信息
1.缓存默认情况下不启用。如果你想加载的图像会在内存或磁盘上的缓存，那么你应该在DisplayImageOptions启用缓存这种方式：
```
DisplayImageOptions defaultOptions = new DisplayImageOptions.Builder()
            ...
            .cacheInMemory(true)
            .cacheOnDisc(true)
            ...
            .build();
ImageLoaderConfiguration config = new ImageLoaderConfiguration.Builder(getApplicationContext())
            ...
            .defaultDisplayImageOptions(defaultOptions)
            ...
            .build();
ImageLoader.getInstance().init(config);
```
