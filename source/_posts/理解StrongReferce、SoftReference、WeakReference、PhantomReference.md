title: 理解StrongReferce、SoftReference、WeakReference、PhantomReference
date: 2015-03-18 16:22:58
categories: Android
tags: [引用]
---
JDK 1.2版本引入了强、软、弱、虚引用的概念
<!--more-->
#StrongReference
- 修饰的对象，JVM宁愿抛出OOM也不回收对象
 
#SoftReference
- 内存不足时，才会回收对象

#WeakReference
- 无论内容是否足够，垃圾回收扫描到，他就会被回收
```java
@Test  
public void weakReference() {   
     Object referent = new Object();   
     WeakReference<Object> weakRerference = new WeakReference<Object>(referent);   
  
     assertSame(referent, weakRerference.get());   
       
     referent = null;   
     System.gc();   
       
    /**
      * 一旦没有指向 referent 的强引用, weak reference 在 GC 后会被自动回收
      */  
     assertNull(weakRerference.get());   
}   
```

#PhantomReference
- 相对于完全没有引用