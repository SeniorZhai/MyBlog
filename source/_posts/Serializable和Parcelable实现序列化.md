title: Serializable和Parcelable实现序列化
date: 2014-09-01 10:48:40
categories: Android
tags: [Serializable,Parcelable,序列化]
---
序列化是为了将对象转换成字节序列保存到本地或者在网络中、进程或Activity间传递对象。
<!--more-->
Android中自定义的对象序列化可以选择`Parcelable`或者`Serializable`来解决。

##区别
1. 在使用内存时，Parcelable比Serializable的性能高
2. Serializable在序列化的过程中产生大量的临时变量，从而引起频繁的GC
3. Parceable不能使用在数据存储在磁盘的情况
4. Serialzable接口是Java SE支持的，Parcelable是Android特有的功能

##实现
- Serializable的实现与使用
```java
public class Box implements Serializable
{
	private int width;
	private int height;

	public void setWidth(int width){
		this.width = width;
	}

	public void setHeight(int height){
		this.height = height;
	}

	public static void main(String[] args){
		Box box = new Box();
		box.setWidth(50);
		box.setHeight(30);
		try{
			FileOutputStream fs = new FileOutputStream("foo.ser");     
          	ObjectOutputStream os =  new ObjectOutputStream(fs);     
            os.writeObject(myBox);     
           	os.close();     
         }catch(Exception ex){     
            ex.printStackTrace();     
         }     
	}
}
```
- Parcelable
实现Parcelable接口的实例，可以将自身的数据信息写入一个Parcel对象，也可以从Parcel中恢复到对象的状态。
Parcel提供了一系列的方法帮助写入数据与读取数据
1. obtain()：在池中获取一个新的Parcel
2. dataSize()：得到当前Parcel对象的实际存储空间
3. dataPosition()：获取当前Parcel对象的偏移量
4. setDataPosition()：设置当前Parcel对象的偏移量
5. recyle()：清空、回收当前Parcel对象的内存
6. writeXxx()：向当前Parcel对象写入数据，具有多种重载
7. readXxx()：从当前Parcel对象读取数据，具有多种重载
简而言之，Parcelable通过writeToParcel()方法，对复杂对象的数据写入Parcel的方法进行对象序列化，需要的时候，通过定义的静态属性CREATOR.createFromParcel()进行反序列化的操作。Parcelable对Parcel进行包装，其内部就是通过Parcel进行序列化与反序列化
- 实现Parcelable接口
Parcelable必须要实现的抽象方法：
	+ abstract int describeContents():返回一个位掩码，表示一组特殊对象类型的Parcelable，一般返回0即可
	+ abstract void writeToParcel(Parcel dest,int flags):实现对象的序列化，通过Parcel的一系列writeXxx()方法序列化对象
	+ abstract T createFromParcel(Parcel source):通过source对象，根据writeToParcel()方法序列化的数据，反序列化一个Parcelable对象
	+ abstract T[] newArray(int size):创建一个新的Parcelable对象的数组
类中定义一个名为`CREATOR`类型为Parcelable.Create<T>的泛型静态属性，实现对象的反序列化
```java
@Override
public int describeContents() {
	return 0;
}
@Override
public void writeToParcel(Parcel dest,int flags) {
	// 序列化
	dest.writeInt(id);
	dest.writeString(msgText);
	dest.writeString(fromName);
	dest.writeString(fromName);
	dest.writeString(date);
}
@Override
public static final Parcelable.Creator<Message> CREATOR = new Creator<Message>() {
@Override
public Message[] new Array(int size){
	return new Message[size];
}
@Override
public Message createFromParcel(Parcel source){
	// 反序列化 顺序要与序列化时相同
	return new Message(source.readInt(),source.readString(),source.readString()source.readString());
}
```