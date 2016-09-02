title: Fragment
date: 2014-09-04 13:22:58
categories: Android
tags: [Fragment,动态交互]
---
Fragment用于创建动态的、多窗口的交互体验
<!--more-->
## 创建一个Fragment
- 必须重写`onCreateView()`回调方法来定义布局
```java
public class ArticleFragment extends Fragment {
	@Override
	public View onCreateView(LayoutInflater inflater,ViewGroup container,Budle savedInstanceState) {
		return inflater.inflate(R.layout.article_view,container,false);
	}
}
```
- 当activity的onPause()方法被调用时，它里面的所有fragment的onPause()方法都会被触发
## 用XML将fragment添加到Activity
- frament是可以重用的，每一个Fragment的实例都必须与一个FragmentActivity关联
```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="horizontal"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent">

    <fragment android:name="com.example.android.fragments.HeadlinesFragment"
              android:id="@+id/headlines_fragment"
              android:layout_weight="1"
              android:layout_width="0dp"
              android:layout_height="match_parent" />

    <fragment android:name="com.example.android.fragments.ArticleFragment"
              android:id="@+id/article_fragment"
              android:layout_weight="2"
              android:layout_width="0dp"
              android:layout_height="match_parent" />
</LinearLayout>
```
- FragmentActivity是Support Library提供的一个特殊的Activity，用来在API11版本以下的系统处理Fragment，如果版本大于11，可以使用普通的Activity
- 使用V7 appcompat library时，activity应该改为继承ActionBarActivity
> 注：使用XML布局的方式将Fragment添加到Activity时，Fragment是不能被动态移除的
## 动态添加
FragmentManager类提供了方法，在Activity运行时能够对Fragment进行添加、移除、替换
- 为了执行Fragement的增加或移除操作，必须使用`FragmentManager`创建一个`FragmentTransaction`对象，它提供了增加、移除、替换以及其他一些操作的APIs
- 运行Fragment必须有一个容器View，Activity必须提供
```xml
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
	android:id="@+id/fragment_container"
	android:layout_width="match_parent"
	android:layout_height="match_parent" />
```
- 在Activity中，使用getSupportFragmentManager()方法获取FragmentManage对象，然后调用beginTransaction()方法创建FragmentTransaction对象，然后调用add()方法天剑fragment
- 使用同一个FragmentTansaction进行多次Fragment事务，完成操作必须调用commit()方法
```java
public class MainActivity extends FragmentActivity {
	@Override
	public void onCreate(Bundle saveInstanceState)
	{
		super.onCreate(saveInstanceState);
		setContentView(R.layout.news_articles);
		if(findViewById(R.id.fragment_container) != null){
			if (saveInstanceState != null) {
				return;
			}
		}
		HeadlinesFragment firstFragment = new HeadelinesFragment();
		firstFragment.setArguments(getIntent().getExtras());
		getSupportFragmentManager().beginTransaction().add(R.id.fragment_container,firstFragment).commit();
	}
}
```
## Fragment替换
- 使用`replace()`方法来替换Fragment
- 为了向后导航与撤销，在FragemntTransaction提交前调用`addToBackStack()`方法
> 当移除或替换一个Fragment并把它放回到返回站中时，被移除的Fragment的生命周期是stopped，当用户返回重新恢复这个Fragment，它的生命周期的restarts。如果没有放入返回栈，移除或替换时，它就被destoryed了
```java
ArticleFragement newFragment = new AricleFragment();
Bundle args = new Bundle();
args.putInt(ArticleFragment.ARG_POSITION,postion);
newFragment.setArguments(args);
FramentTransaction transaction = getSupportFragmentManager().beginTransacation();
transaction.replace(R.id.fragment_container,newFragment);
transaction.commit();
```
- addToBackStack()方法提供了一个可选的String参数为事务指定一个唯一的名字，为了使用`FragmentManager.BackStackEntry`APIs的一些高级Fragment的操作作准备
## Fragment之间的交互
为了更好的实现逻辑，Fragmet之间的交互需要通过它们关联的Activity，Fragment之间不能直接交互
### 定义一个接口
- 为了让Fragment与Activity交互，可以在Fragment类中定义一个接口，在Activity中实现这个接口，Fragment在透明度生命周期的`onAttach`方法中捕获接口的实现，通过调用接口方法来与Activity交互
```java
public class HeadlinesFragment extends ListFragment {
	OnHeadlineSelectedListener mCallback;

	public interface onHeadlineSelectedLinstener {
		public void onArticleSelected(int position);	
	}

	@Override
	public void onAttach(Activity activiy) {
		super.onAttach(activity);
		try {
			mCallback = (OnHeadlineSelectedListener)activity;
		} catch (ClassCastException e) {
			throw new ClassCastException(activity.toString() + " must implement OnHeadlineSelectedListener");
		}
	}
	// .... 列表被点击，传递给父Activity
	@Overrid
	public void onListItemClick(ListView l,View v,int position,long id) {
		mCallback.onArticleSelected(position);
	}
}
```
### 实现接口
```
public class MainActivity extends FragmentActivity implements HeadlinesFragments.onHeadlineSelectedLinstener {
	public void onAricleSelected(int postion) {
		// 处理部分
	}
}
```
### 传递消息给Fragment
- 宿主Activity通过`findFragmentById()`方法来获取`Fragment`实例，然后调用Fragment的public方法来向Fragment来传递消息
```
public claa MainActivity extends FragmentActivity implements HeadlinesFragments.onHeadlineSelectedLinstener {
	//...承接上部分
	public void onArticleSelected(int position) {
        ArticleFragment articleFrag = (ArticleFragment)getSupportFragmentManager().findFragmentById(R.id.article_fragment);

        if (articleFrag != null) {
            articleFrag.updateArticleView(position);
        } else {
            ArticleFragment newFragment = new ArticleFragment();
            Bundle args = new Bundle();
            args.putInt(ArticleFragment.ARG_POSITION, position);
            newFragment.setArguments(args);

            FragmentTransaction transaction = getSupportFragmentManager().beginTransaction();

            transaction.replace(R.id.fragment_container, newFragment);
            transaction.addToBackStack(null);

            transaction.commit();
        }
    }
}
```