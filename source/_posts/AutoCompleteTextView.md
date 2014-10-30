title: AutoCompleteTextView
date: 2014-10-29 14:11:24
categories:
tags:
---
<!--more-->
AutoCompleteTextView工作流程依赖Adapter和Filter，一个处理视图(BaseAdapter)，一个是数据过滤(Filterable)

Filter有两个重要方法：
1. protected FilterResults performFiltering(CharSequence prefix)
	定制过滤策略，根据输入的prefix对数据进行过滤，并组装成FilterResults结果返回
2. protected void publisgResults(CharSequence constraint,FilterResults results)
	发布结果，把上面方法的结果按照一定的要求处理后，通知Adapter进行数据视图刷新