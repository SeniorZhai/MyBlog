title: RecyvlerView
date: 2014-11-26 21:41:12
categories: Android
tags: [Meterial Design,RecyclerView]
---
RecyclerView是android-support-v7-21中新增的一个Widgets，是ListView的升级版，更加先进和灵活。
<!--more-->
使用RecylerVIew组件，需要指定一个Adapter和布局管理器，布局管理器定位item在RecyclerView中的位置，Adapter负责重用子视图、回收视图，避免View的创建和findViewById()的使用，提高性能。
RecylerView提供了以下布局管理器:
- LinearLayoutManager 线性布局，纵向或者横向滚动的列表视图
- GirdLayoutManager 显示item为网格布局
- StaggeredGridLayoutManager 显示item为交错的网格布局
![](/img/14112601.png)
##使用
在布局中定义
```xml
 <android.support.v7.widget.RecyclerView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:scrollbars="vertical"
        android:id="@+id/recyclerview"/>
```
在Java代码中设置Adapter和数据
```java
 	mRecyclerView = (RecyclerView)findViewById(R.id.recyclerview);
    // 设置高性能模式，必须在确定内容大小不变的情况下设置
    mRecyclerView.setHasFixedSize(true);
    // 设施线性布局
    mLayoutManager = new LinearLayoutManager(this);
    mRecyclerView.setLayoutManager(mLayoutManager);
    mAdapter = new MyAdapter();
    mRecyclerView.setAdapter(mAdapter);
```
同时编写自己的Adapter
```java
private class MyAdapter extends RecyclerView.Adapter<MyAdapter.ViewHolder>{
    public class ViewHolder extends RecyclerView.ViewHolder {
        public TextView mTextView;
        public ViewHolder(View v){
            super(v);
            mTextView = (TextView)v.findViewById(R.id.info);
        }
    }

    @Override
    public ViewHolder onCreateViewHolder(ViewGroup viewGroup, int i) {
        View v = LayoutInflater.from(viewGroup.getContext()).inflate(R.layout.item_layout,viewGroup,false);
        ViewHolder vh = new ViewHolder(v);
        return vh;
    }

    @Override
    public void onBindViewHolder(ViewHolder viewHolder, int i) {
        viewHolder.mTextView.setText("Android CecyclerView");
    }
       
    @Override
    public int getItemCount() {
        return 100;
    }
}
```
item的布局
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    xmlns:card_view="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent" android:layout_height="match_parent">
    <android.support.v7.widget.CardView
        xmlns:card_view="http://schemas.android.com/apk/res-auto"
        android:layout_gravity="center"
        android:layout_width="match_parent"
        android:layout_height="200dp"
        card_view:cardCornerRadius="4dp"
        >
        <TextView
            android:layout_gravity="center"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:id="@+id/info"
            />
    </android.support.v7.widget.CardView>
</LinearLayout>
```
RecyclerView默认情况下就有动画，在删除或者增加Ite的时候。如果需要自定义动画，继承RecyclerView.ItemAnimator类，并且使用RecyclerView.setItemAnimator()方法将定义的动画设置到我们的视图中。