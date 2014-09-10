title: DiskLruCache硬盘缓存
date: 2014-09-09 13:41:48
categories: Android
tags: [cache,硬盘缓存]
---
[DiskLruCache](https://gist.github.com/zt1991616/095f26ea9cf96cecbcd2)是一套非Google官方编写，但是获得官方认证的硬盘缓存解决方案。
<!--more-->
通常情况下，应用程序的缓存位置为`/sdcard/Android/data/<application package>/cache`这个路径。当程序被卸载时，此次的数据也会被一并清除。
##打开缓存
创建一个DiskLruCache的实例需要调用它的`open()`方法，接口如下
```java
public static DiskLruCache open(File directory,int appVersion,int valueCount,long maxSize)
```
第一个参数为缓存路径，第二个参数为当前应用程序的版本号，第三个参数指定同一个key可以对应多少个缓存文件(基本使用1)，第四个参数指定最多可以缓存多少字节的数据。
定义一个方法来获取缓存地址，当SD卡不存在或者SD卡不可被移除的时候，获取内部缓存地址`/data/data/<application package>/cache`。uniqueName是来指定不同数据的。
```java
public File getDiskCacheDir(Context context,String uniqueName){
	String cachePath;
	if(Evrionment.MEDIA_MOUNTED.equals(Environment.getExternalStorageState()) || !Environment.isExternalStorageRemoveable()) {
		cachePath = context.getExternalCacheDir().getPath();
	} else {
		cachePath = context.getCacheDir().getPath();
	}
	return new File(cachePath + File.separator + uniqueName);
}
```
获取当前应用程序版本号的方法
```java
public int getAppVersion(Context context){
	try{
		PackageInfo info = context.getPackageManager().getPackageInfo(context.getPackageName(),0);
		return info.versionCode;
	} catch (NameNotFoundException e) {
		e.printStackTrace();
	}
	return 1;
}
```
> 注：每当版本号改变，缓存路径下存储的所有数据都会被清除掉。
打开DiskLruCache的方法则为：
```java
DiskLruCache mDiskLruCache = null;
try {
	File cacheDir = getDiskCacheDir(context,"bitmap");
	if (!cacheDir.exists()) {
		cacheDir.mkdirs();
	}
	mDiskLruCache = DiskLruCache.open(cacheDir,getAppVersion(context),1,10*1024*1024);
}catch(IOException e){
	e.printStackTrace();
}
```
##写入缓存
比如写入一张图片，下载图片的方法
```java
private boolean downloadUrlToStream(String urlString,OutputStream outputStream) {
	HttpURLConnection urlConnection = null;
	BufferedOutputStream out = null;
	BufferedInputStream in = null;
	try{
		final URL url = new URL(urlString);
		urlConnection = (HttpURLConection)url.openConnection();
		in = new BufferedInputStream(urlConnection.getInputStream(),8*1024);
		out = new BufferedOutputStream(outputStream,8*1024);
		int b;
		while((b = in.read()) != -1) {
			out.write(b);
		}
		return true;
	} catch (final IOException) {
		e.printStackTrace();
	} finally {
		if(urlConnection != null) {
			urlConnection.disconnect();
		}
		try{
			if(out != null){
				out.close();
			}
			if(in != null){
				in.close();
			}
		} catch(final IOException e) {
			e.printStackTrace();
		}
	}
	return false;
}
```
写入的操作需要借助`DiskLruCache.Editor`这个类来完成，获取方法如下
```java
public Editor edit(String key) throws IOException
```
参数key为缓存文件的文件名，为了使缓存的文件与URL一一对应，可以将URL进行MD5编码，编码后的字符串一定是唯一的。
```java
public String hashKeyForDisk(String key){
	String cacheKey;
	try{
		final MessageDigest mDigest = MessageDigest.getInstance("MD5");
		mDigest.update(key.getBytes());
		cacheKey = bytesToHexString(mDigest.digest());
	} catch (NoSuchAlgorithmException e) {
		cacheKey = String.valueOf(key.hashCode());
	}
	return cacheKey;
}
private String bytesToHexString(byte[] bytes) {
	StringBuilder sb = new StringBuilder();
	for (int i = 0; i < bytes.lengeh ; i++ ) {
		String hex = Integer.toHexString(0xFF & bytes[i]);
		if(hex.length() == 1){
			sb.append('0');
		}
	}
	return sb.toString();
}
```
获取Editor实例
```java
String imagUrl = "https://octodex.github.com/grinchtocat";
String key = hashKeyForDisk(imageUrl);
DiskLruCache.Editor editor = mDiskLruCache.edit(key);
```
有了DiskLruCache.Editor实例后，可以调用newOutputStream()方法来创建一个输出流，传入到downloadUrlToStream()中实现下载写入的功能。注意，在写入操作完成之后，需要调用`commit()`方法提交才能生效，调用`abort()`方法表示放弃此次写入
```
new Thread(new Runnable(){
	@Override
	public void run(){
		try{
			String imagUrl = "https://octodex.github.com/grinchtocat";
			String key = hashKeyForDisk(imageUrl);
			DiskLruCache.Editor editor = mDiskLruCache.edit(key);
			if(editor != null){
				OutputStream outputStream = editor.newOutputStream(0); //设置了valueCount为1，index传0就可以了
				if(downloadUrlToStream(imageUrl,outStream)) {
					editor.commit();
				} else {
					editor.abort();
				}
			}
			mDiskLruCache.flush();
		} catch(IOException e) {
			e.printStackTrace();
		}
	}
}).start();
```
##读取缓存
借助DiskLruCache的get方法可以获取缓存
```java
public synchronized Snapshot get(String key)throws IOException
```
返回Snapshot对象，只需要getInputStream()方法就可以得到缓存文件的输入流
```java
try{
	String imagUrl = "https://octodex.github.com/grinchtocat";
	String key = hashKeyForDisk(imageUrl);
	DiskLruCache.Snapshot snapShot = mDiskLruCache.get(key); 
	if (snapShot != null) {
		InputStream is = snapShot.getInputStream(0);
		Bitmap bitmap = BitmapFactory.decodeStream(is);
		mImage.setImageBitmap(bitmap);
	} catch (IOException e) {
		e.printStackTrace();
	}
}
```
##移除缓存
通过DiskLruCache的remove()方法实现
```java
public synchronized boolean remove(String key) throws IOException
```
remove需要传入key
```java
try{
	String imagUrl = "https://octodex.github.com/grinchtocat";
	String key = hashKeyForDisk(imageUrl);
	mDiskLruCache.remove(key);
}catch(IOException e){
	e.printStackTrace();
}
```
##其他API
1. size()
返回当前缓存路径下所有缓存数据的总字节数，以byte为单位
2. flush()
将内存中操作记录同步到日志文件(journal文件)当中，DiskLruCache的正常工作的前提就需要journal文件中的内容，最好在Activity的onPause()方法中调用flush()方法。
3. close()
关闭DiskLruCache和open方法对应。关闭后不能再调用DiskLruCache中任何操作缓存方法，通常在Activity的onDestroy()方法中调用
4. delete()
将所有DiskLruCache的缓存数据删除