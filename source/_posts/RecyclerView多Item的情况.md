title: RecyclerView多Item的情况
date: 2015-06-19 11:38:56
categories: Android
tags: [RecyclerView]
---
如果你在ListView中实现过多Item功能时，那么你一个不会对下面的方法不陌生
<!--more-->
```java
@Override
public int getItemViewType(int postion){
	return super.getItemViewType(postion);
}

@Override
public int getViewTypeCount(){
	return super.getViewTypeCount();
}
```
其中`getItemViewType`用来返回当前项是哪种类型布局，`getViewTypeCount`返回当前ListView总共多少种类型的布局

如果在`RecyclerView`实现多种Item，只需要实现一个`getItemType`方法，用来返回item的种类
在`onCreateViewHolder`和`onBindViewHolder`方法中，第二个参数就是item的类型
```java
public class MultipeItemAdapter extends RecyclerView.Adapter<RecyclerView.ViewHolder> {
	public static enum ITEM_TYPE {
		ITEM_TYPE_IMAGE,
		ITEM_TYPE_TEXT
	}
	@Override
	public RecylerView.ViewHolder onCreateViewHolder(ViewGroup parent,int viewType){
		if (viewType == ITEM_TYPE.ITEM_TYPE_IMAGE.ordinal()) {
			return new ImageViewHolder();
		} else {
			return new TextViewHolder();
		}
	}		
	@Override
    public void onBindViewHolder(RecyclerView.ViewHolder holder, int position) {
        if (holder instanceof TextViewHolder) {
            ((TextViewHolder) holder).mTextView.setText(mTitles[position]);
        } else if (holder instanceof ImageViewHolder) {
            ((ImageViewHolder) holder).mTextView.setText(mTitles[position]);
        }
    }
    @Override
    public int getItemViewType(int position) {
        return position % 2 == 0 ? ITEM_TYPE.ITEM_TYPE_IMAGE.ordinal() : ITEM_TYPE.ITEM_TYPE_TEXT.ordinal();
    }
    @Override
    public int getItemCount() {
        return mTitles == null ? 0 : mTitles.length;
    }
}
```