title: SerializeUtils
date: 2014-08-08 13:50:45
categories: Android
tags: [Android]
---
序列化工具类，用于序列化对象对文件或从文件反序列化对象
```java
public class Serialization(String filePath) {
	public static Object deserialization(String filePath){	
		ObjectInputStream in = null;
		try {
			in = new ObjectInputStream(new FileInputStream(filePath));
			Object o = in.readOject();
			in.close();
			return o;
		} catch (FileNotFoundException e) {
			throw new RuntimeException("FileNotFoundException occurred. ", e);
		} catch (ClassNotFoundException e) {
            throw new RuntimeException("ClassNotFoundException occurred. ", e);
        } catch (IOException e) {
            throw new RuntimeException("IOException occurred. ", e);
        } finally {
            if (in != null) {
                try {
                    in.close();
                } catch (IOException e) {
                    throw new RuntimeException("IOException occurred. ", e);
                }
            }
        }
	}
	
	public static void serialization(String filePath,Object obj) {
		ObjectOutputStream out = null;
		try {
			out = new ObjectOutputStream(new fileOutputStream(filePath));
			out.writeObject(obj);
			out.close();
		}  catch (FileNotFoundException e) {
            throw new RuntimeException("FileNotFoundException occurred. ", e);
        } catch (IOException e) {
            throw new RuntimeException("IOException occurred. ", e);
        } finally {
            if (out != null) {
                try {
                    out.close();
                } catch (IOException e) {
                    throw new RuntimeException("IOException occurred. ", e);
                }
            }
        }
    }
}
```