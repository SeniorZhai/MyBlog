title: SwipRefreshLayout
date: 2014-06-13 13:25:39
categories: Android
tags: [Android]
---
SwipRefreshLayout是Google官方推出的一款下拉刷新组件，可以实现Google Now上的下拉刷新效果。
SwipRefreshLayout只能有一个直接子View，且该组件必须是支持下拉刷新的，比如ListView和ScrollView

## Demo示例

activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent" >

    <android.support.v4.widget.SwipeRefreshLayout
        android:id="@+id/swipe_refresh"
        android:layout_width="match_parent"
        android:layout_height="match_parent" >

        <ListView
            android:id="@+id/listview"
            android:layout_width="match_parent"
            android:layout_height="match_parent" >
        </ListView>
    </android.support.v4.widget.SwipeRefreshLayout>

</LinearLayout>
```

listview_item.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content" >

    <TextView
        android:id="@+id/item_name"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_margin="15dp"
        android:gravity="center"
        android:singleLine="true"
        android:textSize="16sp"
        android:textStyle="bold" />
</RelativeLayout>
```

```java
public class MainActivity extends Activity implements SwipeRefreshLayout.OnRefreshListener{
	private SwipeRefreshLayout swipeLayout;
	private ListView listView;
	private ListViewAdapter adapter;
	private List<ItemInfo> infoList;

	@Override
	protected void onCreate(Bundle savedInstanceState){
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);

		swipeLayout = (SwipeRefreshLayout)this.findViewById(R.id.swipe_refresh);
		swipeLayout.setOnRefreshListener(this);

		swipeLayout.setColorScheme(android.R.color.holo_red_light,android.R.color.holo_green_light,android.R.color.holo_blue_bright,android.R.color.orange_light);

		infoList = new ArrayList<ItemInfo>();
		Item info = new ItemInfo();
		info.setName("icon");
		infoList.add(info);
		listView = (ListView) findViewById(R.id.listView);
		adapter = new ListViewAdapter(this,infoList);

	}

	public void onRefresh(){
		new Handler().postDelayed(new Runnable(){
				public void run(){
					ItemInfo info = new ItemInfo();
					info.setName("icon-refresh");
					info.add(info);
					adapter.notifyDataSetChanged();
					swipeLayout.setRefreshing(false);
				}
			},1500);
	}
}
class ItemInfo {

	/**
	 * id
	 */
	private int id;

	/**
	 * name
	 */
	private String name;

	public int getId() {
		return id;
	}

	public void setId(int id) {
		this.id = id;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

}
class ListViewAdapter extends ArrayAdapter<ItemInfo> {
	
	private LayoutInflater inflater;
	
	public ListViewAdapter(Context context, List<ItemInfo> list) {
		super(context, 0, list);
		inflater = LayoutInflater.from(context);
	}
	
	@Override
	public View getView(int position, View convertView, ViewGroup parent) {
		ItemInfo info = getItem(position);
		
		if (convertView == null) {
			convertView = inflater.inflate(R.layout.item_listview, null);
		}
		
		TextView name = (TextView) convertView.findViewById(R.id.item_name);
		name.setText(info.getName());
		
		return convertView;
	}

}
```
