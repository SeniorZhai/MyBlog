title: 判断ListView是否在顶端或者在底部
date: 2014-07-30 13:43:44
categories: Android
tags: [Android]
---
```java
listView.setOnScrollListener(new OnScrollListener(){
	@Override
	public void onScrollStateChanged(AbsListView view,int scrollState){

	}

	@Override
	public void onScoll(AbsListView view,int firstVisibleItem,int visibleItemCount,int totallItemCout){
		if(firstVisibleItem == 0){
			// 滑到顶部
		}
		if(visibleItemCount+firstVisibleItem == totalItemCount) {
			// 滑到底部
		}
	}
})
```