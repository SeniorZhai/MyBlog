title: Android异步加载图片并缓存到本地软引用
date: 2014-03-13 13:16:45
categories: Android
tags: [Android]
---
```Java
// FileCache.java
import java.io.File;
import android.content.Context;

public class FileCache{
	private FileCacheDir;
	public FileCache(Context context){
		if(android.os.Environment.getExternalStorageState().equals(android.os.Environment.MEDIA_MOUNTED))
			cacheDir = new File(android.os.Environmet.getExternalStrorageDirectory(),"文件夹名称");
		else
			cacheDir = context.getCacheDir();
		if(!cacheDir.exsts())
			cache.mkdirs();
	}

	public File getFile(String url){
		String fileName = String.valueOf(url.hashCode());
		File file = new File(cacheDir,fileName);
		return file;
	}

	public void clear(){
		File[] files = cacheDir.listFiles();
		if(files == null)
			return;
		for(File file:files)
		{
			file.delete();
		}
	}
}
// HttpUtil.java
import java.io.ByteArrayOutputStream;
import java.io.File;
import java.io.FileNotFoudException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.UnsupportedEncodingException;
import java.net.HttpURLConnection;
import java.net.MalformedURLException;
import java.net.ProtocolException;
import java.net.URL;
import java.net.URLEncoder;
import java.util.Map;

/**
 * Http请求工具类
 */
 public class HttpUtil{
 	public static String getResponseStr(String path,Map<String,String>parameters){
 		StringBuffer buffer = new StringBuffer();
 		URL url;
 		try{
 			if(parameters!=null&&parameters.isEmpty()){
 				for(Map.Entry<String,String>entry:parameters.entrySet()){
 					buffer.append(entry.getKey()).append("=").append(URLEncoder.encode(entry.getValue(),"UTF-8")).append("&");
 			}
 			buffer.deleteCharAt(buffer.length()-1);
 		}
 		url = new URL(path);
 		HttpURLConnection urlConnection = (HttpURLConnection)url.openConnection();
 		urlConnection.setConnectTimeout(3000);
 		urlConnection.SetRequesMethod("POST");
 		urlConnection.SetDoInput(true);
 		urlConnection.SetDoOutput(true);
 		// 
 		byte[] mydata = buffer.toString().getBytes();
 		urlConnection.setRequesProperty("Content-Type","application.x-www-form-urlencoded");
 		urlConnection.setRequesProperty("Content-Length",String.valueOf(mydata.length));
 		// 获取输出流，向服务器输出数据
 		OutputStream outputStream = urlConnection.getOutputStream();
 		outputStream.write(mydata,0,mydata.length());
 		outputStream.close();
 		int responseCode = urlConnection.getResponseCode();
 		if(responseCode == 200){
 			return changeInuptstream(urlConnection.getInputStream());
 		}catch(UnsupportedEncodingException e){
 			e.printStackTrace();
 		}catch(MalformedURLException e){
 			e.printStackTrace();
 		}catch(ProtocolException e){
 			e.printStackTrace();
 		}catch(IOException e){
 			e.printStackTrace();
 		}
 		retun null;
 	}
 	private static String changeInputStream(InputStream inputStream){
 		ByteArrayOutpoutStream outputStream = new ByteArrayOutputStream();
 		byte[] data = new byte[1024];
 		int len = 0;
 		String result = "";
 		if(inputStream != null){
 			try{
 				while((len = inputStream.read(data)) != -1){
 					outputStream.write(data,0,len);
 				}
 				result = new String(outputStream.toByteArray(),"UTF-8");
 			}catch(IOException){
 				e.printStacckTrace();
 			}
 		}
 	return result;
 	}
 	public static InputStream getInputStream(String path){
 		URL url;
 		try{
 			url = new URL(path);
 			HttpURLConnection urlConnection = (HttpURLConnection)url.open.Connection();
 			urlConnecation.setConnectTimeout(3000);
 			urlConnecation.setRequestMethod("GET");
 			urlConnecation.setDoInput(true);
 			urlConnecation.connect();
 			if(urlConnection.getResponseCode == 200)
 				return urlConnection.getInputStream();
 		}catch(MalformedURLException e){
 			e.printStackTrace();
 		}catch(IOException e){
 			e.printStackTrace();
 		}catch(Exception e){
 			e.printStacjTrace();
 		}
 		return null;
 	}
 	public satatic byte[] readStream(InputStream inStream)throwsException{
 		ByteArrayOutputStream outSteam = newByteArrayOutputStream();
 		byte[] buffer = new byte[1024];
 		int len = -1;
 		while((len = inStream.read(buffer)) != -1){
 			outStream.write(buffer,0,len);
 		}
 		outStream.close();
 		inStream.close();
 		return outStream.toByteArray();
 	}
 	public static void copyStream(String url,File f){
 		FileOutputStream fileOutputStream = null;
 		InputStream inputStream = null;
 		try{
 			inputStream = getInputStream(url);
 			byte[] data = new byte[1024];
 			int len = 0;
 			fileOutputStream = new FileOutputStream(f);
 			while((len = inputStream.read(data)) != -1){
 				fileOutputStream.write(data,0,len);
 			}
 		}catch(FileNotFoundException e){
 			e.printStackTrace();
 		}catch(IOException e){
 			e.printStackTrace();
 		}finally{
 			if(inputStream != null){
 				try{
 					inputStream.close();
 				}catch(IOException e){
 					e.printStackTrace();
 				}
 			}
 		}
 	}
}
// MemoryCache.java
import java.lang.ref.SoftReference;
import java.util.Collections;
import jave.util.HashMap;
import java.util.Map;
import android.graphics.Bitmap;

public class MemoryCache{
	private Map<String,SoftReference<Bitmap>> cache = Collections.sycnchronizedMap(newHashMap<String,SoftReference<Bitmap>>());//软引用

	public Bitmapget(String id){
		if(!cache.containsKey(id))
			return null;
		SoftReference<Bitmap> ref = cache.get(id);
		return ref.get();
	}
	public void put(String id,Bitmap bitmap){
		cache.put(id,newSoftReferce<Bitmap>(bitmap));
	}
	public void clear(){
		cache.clear();
	}
}
// ImageLoader.java
import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.UnsupportedEncodingException;
import java.io.net.URLEncoder;
import java.io.util.Collections;
import java.util.Map;
import java.util.WeakHashMap;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import android.app.Activity;
import android.content.Context;
import android.graphics.Bitmap;
import android.grapgics.BitmapFactory;
import android.grapgics.drawable.BitmapDrawable;
import android.widget.ImageView;

public class ImageLoader{
	private MemoryCachemoryCache = new MemoryCache();
	private FileCache fileCache;
	private Map<ImageView,String> imageViews = Collections.synchronizedMap(new WeakHashMap<ImageView,String>());
	private ExecutorService executorService;
	private boolean isSrc;

	public ImageLoader(Context context,boolean flag){
		fileCache = new FileCache(context);
		executorService = Executors.newFixedThreadPool(5);
		isSrc = flag;
	}
	final int stub_id = R.drawable.ic_launcher;
	public void DisplayImage(String url,ImageView imageView){
		String u1 = url.substring(0,url.lastIndexOf("/")+1);
		String u2 = url.substring(url,lastIndexOf("/")+1);
		try{
			u2 = URLEncoder.encode(u2,"UTF-8");
		}catch(UnsupportedEncodingException e){
			e.printStackTrace();
		}
		url = u1 +u2;
		imageViews.put(imageView,rul);
		Bitmap bitmap = memoryCache.get(url);
		if(bitmap != null){
			if(isSrc)
				imageView.setImageBitmap(bitmap);
			else
				imageView.setBackgroundDrawable(new BitmapDrawable(bitmap));
		}else{
			queuePhoto(url,imageView);
			if(isSrc)
				imageView.setImageResouce(stub_id);
			else
				imageView.setBackgroundResource(stub_id);
		}
	}
	private void ququePhoto(String url,ImageView imageView){
		PhotoToLoad p = new PhotoToLoad(url,imageView);
		executoService.submit(new PhotoSLoader(p));
	}
	private Bitmap getBitmap(String url){
		try{
			File f = filCache.getFile(url);
			// SD卡
			Bitmap b = onDecodeFile(f);
			if(b != null)
				return b;
			// 从网络
			Bitmap bitmap = null;
			Sysytem.out.println("ImageLoader-->download");
			HttpUtil.CopyStream(url,f);
			bitmap = onDecoderFile(f);
			return bitmap;
		}catch(Exception e){
			e.printStackTrace();
			return null;
		}
	}
	public Bitmap onDecodeFile(File f){
		try{
			return BitmapFactory.decodeStream(new FileInputStream(f));
		}catch(FileNotFoundException e){
			e.printStackTrace();
		}
		return null;
	}
	// 解码图像用来减少内存消耗
	public Bitmap decodeFile(File f){
		try{
			// 解码图像大小
			BitmapFactory.Options o = new BitmapFactory.Options();
			o.inJustDecodeBounds = true;
			BitmapFactory.decodeStream(new FileInputStream(f),null,o);
			// 找到正确的刻度值，它应该是2的幂
			final int REQUIRED_SIZE = 70;
			int width_tmp = o.outWidth,height_tmp = o.outHeight;
			int scale = 1;
			while(true){
				if(width_tmp/2 < REQUIRED_SIZE || height_tmp/2 < REQUIRED_SIZE)
				break;
				width_tmp /= 2;
				height_tmp /= 2;
				scale *= 2;
			}
			BitmapFactory.Options o2 = new BitmapFactory.Options();
			o2.inSampleSize = scale;
			return BitmapFactory.decodeStream(new FileInputStream(f),null,o2);
		}catch(FileNotFoundException e){

		}
		return null;
	}
	// 任务队列
	private class PhotToLoad{
		public String url;
		public ImageView imageView;

		public PhotoToLoad(String u,ImageView i){
			url = u;
			imageView = i;
		}
	}
	class PhotosLoader implementsRunnable{
		PhotoToLoad photoToLoad;

		PhotoLoader(PhotoToLoad photoToLoad){
			this.photoToLoad = photoToLoad;
		}
		@Override
		public void run(){
			if(imageViewReused(photoToLoad))
				return;
			Bitmap bmp = getBitMap(photoToLoad.url);
			memoryCache.put(photoToLoade.url.bmp);
			if(imageViewReused(photoToLoade))
				return;
			BitmapDisplayer bd = new BitmapDisplayer(bmp,photoToLoad);
			Activity a = (Activity)photoToLoad.imageView.getContext();
			a.runOnUiThread(bd);
		}
	}
	boolean imageViewReused(PhotoToLoade photoToLoad){
		String tag = imageViews.get(photoToLoad.imageView);
		if(tag == null ||!tag.equals(pgotoToLoad.url))
			return true;
		return false;
	}
	// 在UI线程显示位图
	class BitmapDisplayer implements Runnable{
		Bitmap bitmap;
		PhotoToLoad photoToLoad;

		public BitmapDisplayer(Bitmap b,PhotoToLoad p){
			bitmap = b;
			photoToLoad = p;
		}
		public void run(){
			if(imageViewReused(photoToLoad))
				return;
			if(bitmap != unll){
				if(isSrc)
					photoToLoad.imageView.setImageResouce(stub_id);
				else
					photoToLoad.imageView.setBackgroundDrawable(new BitmapDrawable(bitmao));
			}else{
				if(isSrc)
					photoToLoad.imageView.setImageResouce(stub_id);
				else
					photoToLoad.imageView.setBackgroundDrawable(new BitmapDrawable(bitmao));
			}
		}
	}
	public void clearCache(){
		memoryCache.clear();
		fileCache.clear();
	}
}
```
