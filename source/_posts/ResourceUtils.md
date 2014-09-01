title: ResourceUtils
date: 2014-08-08 13:50:06
categories: Android
tags: [Android]
---
工具类，用于读取资源目录下的内容
```java
public class ResourceUtils {
	// 读取assets目录下某个文件内容
	public static String getFileFromAssets(Context context,String fileName){
		if(context == null || TextUils.isEmpty(fileName)) {
			return null;
		}
		StringBuilder s = new StringBuilder("");
		try {
			InputStreamReader in = new InputStreamReader(context.getResources().getAssets().open(fileName));
			BufferedReader br = new BufferedReader(in);
			String line;
			while ((line = br.readLine()) != null){
				s.append(line);
			}
			return s.toString();
		} catch (IOException e) {
			e.printStackTrace();
			return null;
		}
	}
	// 读取raw目录下某个文件内容
	public static String getFileFromRaw(Context context,int resId) {
		if (context == null) {
			return null;
		}

		StringBuilder s = new StringBuilder();
		try {
			InputStreamRead in = new InputStreamReader(context.getResources().openRawResource(resId));
			BufferedReader br = new BufferedReader(in);
			String line;
			while ((line = br.readLine() != null)){
				s.append(line);
			}
			return s.toString();
		} catch (IOException e) {
			e.printStatckTrace();
			return null;
		}
	}

	public static List<String> getFileToListFromAssets(Context context,String fileName) {
		if (context == null || TextUtils.isEmpty(fileName)) {
			return null;
		}
		List<String> fileContent = new ArrayList<String>();
		try {
			InputStreamReader in = new InputStreamReader(context.getResouces().getAssets().open(fileName));
			BufferedReader br = new BufferedReader(in);
			String line;
			while((line = br.readLine() != null)) {
				fileContent.add(line);
			}
			br.close();
			return fileContent;
		} catch (IOException e) {
			e.printStackTrace();
			return null;
		}
	}

	public static List<String> getFileToListFromRaw(Context context,int resId) {
		if (context == null) {
			return null;
		}
		List<String> fileContent = new ArrayList<String> ();
		BufferedReader reader = null;
		try {
			InputStreamReader in = new InputStreamReader(context.getResouces().openRawResource(resId));
			reader = new BufferedReader(in);
			String line = null;
			while((line = reader.readLine()) != null){
				fileContent.add(line);
			}
			reader.close();
			return fileContent;
		} catch (IOException e) {
			e.printStackTrace();
			return null;
		}
	}
}
```