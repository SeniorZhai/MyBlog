title: Android ViewFinder
date: 2014-08-26 14:09:32
categories: Android
tags: [Android]
---
<!--more-->
在基类BaseActivity中加入如下函数
```java
public final <E extends View> E getView (int id) {
	try {
		return (E) findViewById(id);
	} catch (ClassCastException ex) {
		Log.e(TAG,"Could not cast View to concrete class.",ex);
		throw ex;
	}
}
```
使用ViewFinder类，封装了常用的方法。可以在Activity或View中调用。
```java
public class ViewFinder {
	private static interface FindWrapper {
		View findViewById(int id);
		Resources getResources();
	}

	private static class WindowWrapper implements FindWapper {
		private final Window window;

		WidowWrapper(final Window window) {
			this.window = window;
		}

		public View findViewById(final int id) {
			return window.findViewById(id);
		}

		public Resources getResources() {
			return window.getContext().getResouces();
		}
	}

	private static class ViewWrapper implements FindWrapper {
		private final View view;

		ViewWrapper(final View view) {
			this.view = view;
		}

		public View findViewById(int id) {
			return view.findViewById(id);
		}

		public Resources getResources() {
			return view.getResources();
		}
	}

	private final FindWrapper wrapper;

	public ViewFinder(final View view) {
		wrapper = new ViewWrapper(view);
	}

	public ViewFinder(final Window window) {
		wrapper = new WindowWrapper(window);
	}

	public ViewFinder(final Activity activity) {
		this(activity.getWindow());
	}

	public <V extends View> V find (final int id) {
		return (V) wrapper.findViewById(id);
	}

	public ImageView imageView(final int id) {
		return find(id);
	}

	public CompoundButton commpoudButton(final int id) {
		return find(id);
	}

	public TextView textView(final int id) {
		return find(id);
	}

	public TextView setText(final int id, final CharSequence content) {
		final TextView text = find(id);
		text.setText(content);
		return text;
	}

	public TextView setText(final int id, final int content) {
		return setText(id, wrapper.getResources().getString(content));
	}

	public View onClick(final int id, final Runnable runnable) {
		return onClick(id, new OnClickListener() {
 
			public void onClick(View v) {
				runnable.run();
			}
		});
	}

	public void onClick(final OnClickListener listener, final int... ids) {
		for (int id : ids)
			find(id).setOnClickListener(listener);
	}

 	public void onClick(final Runnable runnable, final int... ids) {
		onClick(new OnClickListener() {
 
			public void onClick(View v) {
				runnable.run();
			}
		}, ids);
	}

	public ImageView setDrawable(final int id, final int drawable) {
		ImageView image = imageView(id);
		image.setImageDrawable(image.getResources().getDrawable(drawable));
		return image;
	}

	public CompoundButton onCheck(final int id, final OnCheckedChangeListener listener) {
		CompoundButton checkable = find(id);
		checkable.setOnCheckedChangeListener(listener);
		return checkable;
	}

	public CompoundButton onCheck(final int id, final Runnable runnable) {
		return onCheck(id, new OnCheckedChangeListener() {
 
			public void onCheckedChanged(CompoundButton buttonView, boolean isChecked) {
				runnable.run();
			}
		});
	}

	public void onCheck(final OnCheckedChangeListener listener, final int... ids) {
		for (int id : ids)
			compoundButton(id).setOnCheckedChangeListener(listener);
	}

	public void onCheck(final Runnable runnable, final int... ids) {
		onCheck(new OnCheckedChangeListener() {
 
			public void onCheckedChanged(CompoundButton buttonView, boolean isChecked) {
				runnable.run();
			}
		}, ids);
	}
}
```