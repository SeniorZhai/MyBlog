title: SharedPreferce最佳实践
date: 2014-09-11 08:54:08
categories: Android
tags: [SharedPreferce]
---
SharedPreferce作为Android提供的一种保存数据的解决方案，用于保存键值对形式的数据。一本用于存放应用设置，用户数据等。
<!--more-->
常用的类包括
- SharedPreferces 获取建立存储数据
- SharedPreferces.Editor 对数据编辑的接口
- SharedPreferces.OnSharedPreferenChangeListener 数据变化的监听器
## SharedPreferces
主要用于创建SharedPreferences对象，通过SharedPreference获取数据。常用API有：
```java
// 根据名字创建
SharedPreferences preferences = context.getSharedPreferences("shared_name",Context.Mode_PRIVATE);
// 获取变量
preferences.getBoolean("key",defaultValue);
prrferences.get...("key",defaultValue);
// 获取整个键值Map
Map<String,?> all = preferences.getAll();
```
## Editor
SharedPreferences.Editor是用于修改SharedPreferences对象的接口，且Editor做出的修改默认是待处理的，需要使用commit()或者apply()提交修改。
>commit()会更快一点
常用的Api有：
```java
// 获取Editor对象
SharedPreferences.Editor editor = preferences.edit();
// 添加数据
editor.putBoolean("key",value);
editor.put...("key",value);
// 删除数据
editor.remove("key");
// 清空数据
editor.clear();
// 提交数据，返回是否成功
booleanresult = editor.commit();
// 提交数据，与commit相同，但属于异步操作，无返回值
editor.apply();
```
## SharedPreferenChangeListener
用于监听SharedPreferences的数据改变，常用API有
```java
// 注册监听器
preferences.registerOnSharedPreferenceChangeListener(mListener);
// 注销监听器
preferences.unregisterOnSharedPreferenceChangeListener(mListener);
// 监听器
SharedPreferences.OnSharedPreferenceChangeListener mOnSharedPreferenceChangeListener = new SharedPreferences.OnSharedPreferenceChangeListener() {
	@Override
	public void onSharedPreferenceChanged(SharedPreferces sharedPreferences,String key){
		// 实现监听
	}	
};
```
## 性能
- ShredPreferences是单例对象，第一次打开后，之后获取都无需创建，速度很快。
- 当第一次获取数据后，数据会被加载到一个缓存的Map中，之后的读取都会非常快。
- 当由于是XML<->Map的存储方式，所以，数据越大，操作越慢，get、commit、apply、remove、clear都会受影响，所以尽量把数据按功能拆分成若干份。
- App更新后，Preferences不会被移除，在默写情况下，需要创建迁移数据的方案
```java
public class MigrationManager {
	private final static String KEY_PREDERENCES_VERSION = "key_preferences_version";
	private final static int PREFERENCES_VERSION = 2;

	public static void migrate(Context context) {
		SharedPreferences preferences = context.getSharedPreferences("pref",Context.MODE_PRIVATE);
		checkPreferences(preferences);
	}
	// 根据当前App的Preferences版本号，判断是否更新SharedPreferences
	private static void checkPreferences(SharedPreferences thePreferences) {
		 final int oldVersion = thePreferences.getInt(KEY_PREFERENCES_VERSION,1);
		 if(oldVersion < PREFERENCES_VERSION) {
		 	final SharedPreferences.Editor edit = thePreferences.edit();
            edit.clear();
            edit.putInt(KEY_PREFERENCES_VERSION, currentVersion);
            edit.commit();
		 }
	}
}
```
## 存放位置
- `/data/data/YOUR_PACKAGE_NAME/shared_prefs/YOUR_PREFS_NAME.xml` 
- `/data/data/YOUR_PACKAGE_NAME/shared_prefs/YOUR_PACKAGE_NAME_preferences.xml`
## 示例
```java
public class PreferencesManager {

    private static final String PREF_NAME = "com.example.app.PREF_NAME";
    private static final String KEY_VALUE = "com.example.app.KEY_VALUE";

    private static PreferencesManager sInstance;
    private final SharedPreferences mPref;

    private PreferencesManager(Context context) {
        mPref = context.getSharedPreferences(PREF_NAME, Context.MODE_PRIVATE);
    }

    public static synchronized void initializeInstance(Context context) {
        if (sInstance == null) {
            sInstance = new PreferencesManager(context);
        }
    }

    public static synchronized PreferencesManager getInstance() {
        if (sInstance == null) {
            throw new IllegalStateException(PreferencesManager.class.getSimpleName() +
                    " is not initialized, call initializeInstance(..) method first.");
        }
        return sInstance;
    }

    public void setValue(long value) {
        mPref.edit()
                .putLong(KEY_VALUE, value)
                .commit();
    }

    public long getValue() {
        return mPref.getLong(KEY_VALUE, 0);
    }

    public void remove(String key) {
        mPref.edit()
                .remove(key)
                .commit();
    }

    public boolean clear() {
        return mPref.edit()
                .clear()
                .commit();
    }
}
```