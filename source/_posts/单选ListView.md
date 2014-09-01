title: 单选ListView
date: 2014-08-27 18:51:44
categories: Android
tags: [Android]
---
实现单选必须要设置ListView为CHOICE_MODE_SINGLE(listView.setChoiceMode(ListView.CHOICE_MODE_SINGLE)或android:choiceMode="singleChoice")
```java
  private int cur_pos = 0;// 当前显示的一行
  private String[] items_text = { "选项一", "选项二", "选项三", "选项四", "选项五" };
    ...
    final Mydapter dapter = new Mydapter(this);
    ListView listview = (ListView) findViewById(R.id.listView);
    listview.setAdapter(dapter);
    listview.setOnItemClickListener(new OnItemClickListener() {
      @Override
      public void onItemClick(AdapterView<?> arg0, View arg1,
          int position, long id) {
        cur_pos = position;// 更新当前行
        dapter.notifyDataSetChanged();
      }
    });
```
---
```java
public class Mydapter extends BaseAdapter {
    private LayoutInflater inflater;

    public Mydapter(Context context) {
      inflater = (LayoutInflater) context
          .getSystemService(Context.LAYOUT_INFLATER_SERVICE);
    }

    @Override
    public int getCount() {
      return items_text.length;
    }

    @Override
    public Object getItem(int position) {
      return items_text[position];
    }

    @Override
    public long getItemId(int position) {
      return position;
    }

    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
      Log.e("TEST", "refresh once");
      convertView = inflater.inflate(R.layout.item, null, false);
      TextView tv = (TextView) convertView.findViewById(R.id.tv);// 显示文字
      tv.setText(items_text[position]);
      if (position == cur_pos) {// 如果当前的行就是ListView中选中的一行，就更改显示样式
        convertView.setBackgroundColor(R.drawable.channel_list_item_bg_selected);// 更改整行的背景色
      }
      return convertView;
    }
  }
```

[例子](https://github.com/zt1991616/SingleChoiceList/)