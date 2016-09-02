title: StickyListHeadersListView
date: 2014-11-20 13:43:07
categories: Android
tags: [HeadersList,带标题的ListView]
---
[StickyListHeaders](https://github.com/emilsjolander/StickyListHeaders)是一个带标题的ListView的带三方库
<!--more-->
## 使用
在activity或Fragment的xml文件
```xml
<se.emilsjolander.stickylistheaders.StickyListHeadersListView
    android:id="@+id/list"
    android:layout_width="match_parent"
    android:layout_height="match_parent"/>
```
在`onCreate()`或`onCreateView()`中获取StickyListHeadersListView
```java
StickyListHeadersListView stickyList = (StickyListHeadersListView) findViewById(R.id.list);
MyAdapter adapter = new MyAdapter(this);
stickyList.setAdapter(adapter);
```
同时要实现一个Adapter
```java
public class Mydapter extends BaseAdapter implements StickyListHeadersAdapter {
	@Override
	public int getCount(){
		return content.length;
	}

	@Override
    public Object getItem(int position) {
        return content[position];
    }

    @Override
    public long getItemId(int position) {
        return position;
    }

    @Override 
    public View getView(int position, View convertView, ViewGroup parent) {
        ViewHolder holder;

        if (convertView == null) {
            holder = new ViewHolder();
            convertView = inflater.inflate(R.layout.test_list_item_layout, parent, false);
            holder.text = (TextView) convertView.findViewById(R.id.text);
            convertView.setTag(holder);
        } else {
            holder = (ViewHolder) convertView.getTag();
        }

        holder.text.setText(content[position]);

        return convertView;
    }

    @Override 
    public View getHeaderView(int position, View convertView, ViewGroup parent) {
        HeaderViewHolder holder;
        if (convertView == null) {
            holder = new HeaderViewHolder();
            convertView = inflater.inflate(R.layout.header, parent, false);
            holder.text = (TextView) convertView.findViewById(R.id.text);
            convertView.setTag(holder);
        } else {
            holder = (HeaderViewHolder) convertView.getTag();
        }
        //set header text as first char in name
        String headerText = "" + content[position].subSequence(0, 1).charAt(0);
        holder.text.setText(headerText);
        return convertView;
    }

    @Override
    public long getHeaderId(int position) {
        //return the first character of the country as ID because this is what headers are based upon
        return content[position].subSequence(0, 1).charAt(0);
    }

    class HeaderViewHolder {
        TextView text;
    }

    class ViewHolder {
        TextView text;
    }
}
```