title: RecyclerView的使用
date: 2015-01-06 14:42:31
categories:
tags:
---
RecylerView作为ListView，RecylerView标准化了ViewHolder，不同于ListView中复用convertView是复用的，在RecyclerView把ViewHolder作为缓存的单位。
<!--more-->
RecylerView作为一个容器，能够有效的重用和滚动
	- 它为item的定位提供了LayoutManager
	- 为item提供了一个缺省的animations，方便设置动画

##使用
使用RecyclerView需要指定adapter和layoutmanager，且Adapter必须继承`RecyclerView.Adapter`