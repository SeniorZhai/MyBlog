title: Android开发贴士
date: 2015-02-28 10:26:01
categories: Android
tags: [Tips]
---
转至<http://blog.danlew.net/2014/03/30/android-tips-round-up-part-1/>
<!--more-->
[Activity.startActivities()](http://developer.android.com/reference/android/app/Activity.html#startActivities(android.content.Intent[]))——对于从app流的中部启动会非常好。

[TextUtils.isEmpty()](http://developer.android.com/reference/android/text/TextUtils.html#isEmpty(java.lang.CharSequence))——一个普遍适用的简单工具类。

[Html.fromHtml()](http://developer.android.com/reference/android/text/TextUtils.html#isEmpty(java.lang.CharSequence))——格式化Html的快速方法，本人认为它也不是非常快，所以我不是经常用它（我说不经常用它是为了重点突出这句话：请多手动构建Spannable来替换Html.fromHtml），但是它对渲染从web上获取的文字还是很不错的。

[TextView.setError()](http://developer.android.com/reference/android/widget/TextView.html#setError%28java.lang.CharSequence%29)——在验证用户输入的时候用户体验很不错。

[Build.VERSION_CODES](http://developer.android.com/reference/android/os/Build.VERSION_CODES.html)——它不仅仅描述了版本号，还总结了各Android版本的不同特性。

[Log.getStackTraceString()](http://developer.android.com/reference/android/util/Log.html#getStackTraceString(java.lang.Throwable))——方便的日志工具。

[LayoutInflater.from()](http://developer.android.com/reference/android/view/LayoutInflater.html#from%28android.content.Context%29)——简化一系列冗长的getSystemService()调用的简单工具。

[ViewConfiguration.getScaledTouchSlop()](http://developer.android.com/reference/android/view/ViewConfiguration.html#getScaledTouchSlop%28%29)——使用ViewConfiguration中提供的值以保证所有触摸的交互都是统一的。

[PhoneNumberUtils.convertKeypadLettersToDigits](http://developer.android.com/reference/android/telephony/PhoneNumberUtils.html#convertKeypadLettersToDigits%28java.lang.String%29)——使得处理电话号码更方便，很多人都只提供字母，而不是数字。

[Context.getCacheDir()](http://developer.android.com/reference/android/content/Context.html#getCacheDir%28%29)——使用系统提供的缓存目录进行数据缓存，操作非常简单不过很多人不知道怎么使用。

[ArgbEvaluators](http://developer.android.com/reference/android/animation/ArgbEvaluator.html)——处理颜色的渐变。就像[Chris Banes](https://plus.google.com/108078989781026808271/posts/SbieMPqa2by)说的一样，这个类会进行很多自动装箱的操作，所以最好还是去掉它的逻辑自己去实现它。

[ContextThemeWrapper](http://developer.android.com/reference/android/view/ContextThemeWrapper.html)——方便在运行过程中更改主题。

[Space](http://developer.android.com/reference/android/widget/Space.html)——轻量级的视图组件，可以跳过绘制的过程，对于需要占位符的任何场景来说都是很棒的。

[ValueAnimator.reverse()](http://developer.android.com/reference/android/animation/ValueAnimator.html#reverse%28%29)——可以顺畅地取消动画效果，很赞。

[DateUtils.formatDateTime()](http://developer.android.com/reference/android/text/format/DateUtils.html#formatDateTime%28android.content.Context,%20long,%20int%29)——提供区域格式化时间/日期字符串的一站式服务。

[AlarmManager.setInexactRepeating](http://developer.android.com/reference/android/app/AlarmManager.html#setInexactRepeating(int, long, long, android.app.PendingIntent))——通过闹铃分组的方式来节省电量，即使你只调用一个alarm实例，它仍然比较好用（可以确保在使用完毕时自动调用AlarmManager.cancel()。

[Formatter.formatFileSize()](http://developer.android.com/reference/android/text/format/Formatter.html#formatFileSize(android.content.Context, long))——一个区域化的文件大小格式化工具。

[ActionBar.hide()](http://developer.android.com/reference/android/app/ActionBar.html#hide()) /[.show()](http://developer.android.com/reference/android/app/ActionBar.html#show())——可以在actionBar显示或者隐藏的时候进行动画展示。可以在切换到全屏的时候更优雅。

[Linkify.addLinks()](http://developer.android.com/reference/android/text/util/Linkify.html#addLinks(android.text.Spannable, int))——可以控制在Text上添加链接。

[StaticLayout](http://developer.android.com/reference/android/text/StaticLayout.html)——在自定义View中渲染文字的时候很实用。

[Activity.onBackPressed()](http://developer.android.com/reference/android/app/Activity.html#onBackPressed())——方便控制返回按钮，在需要自定义返回键的操作时候，可以用到。

[GestureDetector](http://developer.android.com/reference/android/view/GestureDetector.html)——可以监听动作事件和相关的监听器事件（点击，滚动，滑动等）。比自己实现系统的一些动作事件更简单。

[DrawFilter](http://developer.android.com/reference/android/graphics/DrawFilter.html)——可以让你操作Canvas，即使没有调用draw方法。例如，可以在创建自定义View的时候设置一个DrawFilter，给父View里面的所有View设置反别名。

[ActivityManager.getMemoryClass()](http://developer.android.com/reference/android/app/ActivityManager.html#getMemoryClass())——可以让你清楚知道设备还剩多少内存。在计算怎么设置缓存大小的时候就很有用。

[SystemClock.sleep()](http://developer.android.com/reference/android/os/SystemClock.html#sleep(long))——这个方法在保证一定时间的sleep时很方便，通常我用来进行debug和模拟网络延时。

[ViewStub](http://developer.android.com/reference/android/view/ViewStub.html)——它是一个初始化不做任何事情的View，但是之后可以载入一个布局文件。在慢加载View中很适合做占位符。唯一的缺点就是不支持标签，所以如果你不太小心的话，可能会在视图结构中加入不需要的嵌套。

[DisplayMetrics.density](http://developer.android.com/reference/android/util/DisplayMetrics.html#density)——通过这个方法可以获取屏幕的密度，很多时候需要去掉系统自动缩放精度的功能，但是有时候在控制的时候也很有用（尤其是在自定义View的时候）。

[Pair.create()](http://developer.android.com/reference/android/util/Pair.html#create(A, B))——方便构建类和构造器的方法。

[UrlQuerySanitizer](http://developer.android.com/reference/android/net/UrlQuerySanitizer.html)——使用这个工具可以方便对URL进行检查。

[Fragment.setArguments](http://developer.android.com/reference/android/app/Fragment.html#setArguments%28android.os.Bundle%29)——因为在构建Fragment的时候不能加参数，所以这是个很好的东西，可以在创建Fragment之前设置参数（即使在configuration改变的时候仍然会导致销毁/重建）。

[DialogFragment.setShowsDialog()](http://developer.android.com/reference/android/app/DialogFragment.html#setShowsDialog%28boolean%29)——这是一个很巧妙的方式，DialogFragment可以作为正常的Fragment显示！这里可以让Fragment承担双重任务。我通常在创建Fragment的时候把onCreateView()和onCreateDialog()都加上，就可以创建一个具有双重目的的Fragment。

[FragmentManager.enableDebugLogging()](http://developer.android.com/reference/android/app/FragmentManager.html#enableDebugLogging%28boolean%29)——在需要观察Fragment状态的时候会有帮助。

[LocalBroadcastManager](http://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html)——这个会比全局的broadcast更加安全，简单，快速。像otto这样的Event buses机制对你的应用场景更加有用。

[PhoneNumberUtils.formatNumber()](http://developer.android.com/reference/android/telephony/PhoneNumberUtils.html#formatNumber%28java.lang.String%29)——顾名思义，这是对数字进行格式化操作的时候用的。

[Region.op()](http://developer.android.com/reference/android/graphics/Region.html#op%28android.graphics.Region,%20android.graphics.Region,%20android.graphics.Region.Op%29)——我发现在对比两个渲染之前的区域的时候很实用，如果你有两条路径，那么怎么知道它们是不是会重叠呢？使用这个方法就可以做到。

[Application.registerActivityLifecycleCallbacks](http://developer.android.com/reference/android/app/Application.html#registerActivityLifecycleCallbacks%28android.app.Application.ActivityLifecycleCallbacks%29)——虽然缺少官方文档解释，不过我想它就是注册Activity的生命周期的一些回调方法（顾名思义），就是一个方便的工具。

[versionNameSuffix](http://tools.android.com/tech-docs/new-build-system/user-guide#TOC-Build-Types)——这个gradle设置可以让你在基于不同构建类型的manifest中修改版本名这个属性，例如，如果需要在在debug版本中以”-SNAPSHOT”结尾，那么就可以轻松的看出当前是debug版还是release版。

[CursorJoiner](http://developer.android.com/reference/android/database/CursorJoiner.html)——如果你是只使用一个数据库的话，使用SQL中的join就可以了，但是如果收到的数据是来自两个独立的ContentProvider，那么CursorJoiner就很实用了。

[Genymotion](http://www.genymotion.com/)——一个非常快的Android模拟器，本人一直在用。

[-nodpi](http://developer.android.com/guide/practices/screens_support.html#qualifiers)——在没有特别定义的情况下，很多修饰符（-mdpi，-hdpi，-xdpi等等）都会默认自动缩放assets/dimensions，有时候我们需要保持显示一致，这种情况下就可以使用 -nodpi。

[BroadcastRecevier.setDebugUnregister()](http://developer.android.com/reference/android/content/BroadcastReceiver.html#setDebugUnregister%28boolean%29)——又一个方便的调试工具。

[Activity.recreate()](http://developer.android.com/reference/android/app/Activity.html#recreate%28%29)——强制让Activity重建。

[PackageManager.checkSignatures()](http://developer.android.com/reference/android/content/pm/PackageManager.html#checkSignatures%28java.lang.String,%20java.lang.String%29)——如果同时安装了两个app的话，可以用这个方法检查。如果不进行签名检查的话，其他人可以轻易通过使用一样的包名来模仿你的app。

[Activity.isChangingConfigurations()](http://developer.android.com/reference/android/app/Activity.html#isChangingConfigurations%28%29)——如果在Activity中configuration会经常改变的话，使用这个方法就可以不用手动做保存状态的工作了。

[SearchRecentSuggestionsProvider](http://developer.android.com/reference/android/content/SearchRecentSuggestionsProvider.html)——可以创建最近提示效果的provider，是一个简单快速的方法。

[ViewTreeObserver](http://developer.android.com/reference/android/view/ViewTreeObserver.html)——这是一个很棒的工具。可以进入到VIew里面，并监控View结构的各种状态，通常我都用来做View的测量操作（自定义视图中经常用到）。

[org.gradle.daemon=true](https://www.timroes.de/2013/09/12/speed-up-gradle/)——这句话可以帮助减少Gradle构建的时间，仅在命令行编译的时候用到，因为Android Studio已经这样使用了。

[DatabaseUtils](http://developer.android.com/reference/android/database/DatabaseUtils.html)——一个包含各种数据库操作的使用工具。

[android:weightSum (LinearLayout)](http://developer.android.com/reference/android/widget/LinearLayout.html#attr_android:weightSum)——如果想使用layout weights，但是却不想填充整个LinearLayout的话，就可以用weightSum来定义总的weight大小。

[android:duplicateParentState (View)](http://developer.android.com/reference/android/view/View.html#attr_android:duplicateParentState)——此方法可以使得子View可以复制父View的状态。比如如果一个ViewGroup是可点击的，那么可以用这个方法在它被点击的时候让它的子View都改变状态。

[android:clipChildren (ViewGroup)](http://developer.android.com/reference/android/view/ViewGroup.html#attr_android:clipChildren)——如果此属性设置为不可用，那么ViewGroup的子View在绘制的时候会超出它的范围，在做动画的时候需要用到。

[android:fillViewport (ScrollView)](http://developer.android.com/reference/android/widget/ScrollView.html#attr_android:fillViewport)——在这片文章中有详细介绍文章链接，可以解决在ScrollView中当内容不足的时候填不满屏幕的问题。

[android:tileMode (BitmapDrawable)](http://developer.android.com/guide/topics/resources/drawable-resource.html#Bitmap)——可以指定图片使用重复填充的模式。

[android:enterFadeDuration/android:exitFadeDuration (Drawables)](http://developer.android.com/reference/android/R.attr.html#exitFadeDuration)——此属性在Drawable具有多种状态的时候，可以定义它展示前的淡入淡出效果。

[android:scaleType (ImageView)](http://developer.android.com/reference/android/widget/ImageView.html#attr_android:scaleType)——定义在ImageView中怎么缩放/剪裁图片，一般用的比较多的是“centerCrop”和“centerInside”。

[<merge>](http://developer.android.com/training/improving-layouts/reusing-layouts.html#Merge)——此标签可以在另一个布局文件中包含别的布局文件，而不用再新建一个ViewGroup，对于自定义ViewGroup的时候也需要用到；可以通过载入一个带有标签的布局文件来自动定义它的子部件。

[AtomicFile](http://developer.android.com/reference/android/util/AtomicFile.html)——通过使用备份文件进行文件的原子化操作。这个知识点之前我也写过，不过最好还是有出一个官方的版本比较好。

[ViewDragHelper](https://developer.android.com/reference/android/support/v4/widget/ViewDragHelper.html) ——视图拖动是一个比较复杂的问题。这个类可以帮助解决不少问题。如果你需要一个例子，DrawerLayout就是利用它实现扫滑。Flavient Laurent 还写了一些关于这方面的优秀文章。

[PopupWindow](https://developer.android.com/reference/android/widget/PopupWindow.html)——Android到处都在使用PopupWindow ，甚至你都没有意识到（标题导航条ActionBar，自动补全AutoComplete，编辑框错误提醒Edittext Errors）。这个类是创建浮层内容的主要方法。

[Actionbar.getThemrContext()](https://developer.android.com/reference/android/app/ActionBar.htmlgetThemedContext())——导航栏的主题化是很复杂的（不同于Activity其他部分的主题化）。你可以得到一个上下文（Context），用这个上下文创建的自定义组件可以得到正确的主题。

[ThumbnailUtils](https://developer.android.com/reference/android/media/ThumbnailUtils.html)——帮助创建缩略图。通常我都是用现有的图片加载库（比如，Picasso 或者 Volley），不过这个ThumbnaiUtils可以创建视频缩略图。
译者注：该API从V8才开始支持。

[Context.getExternalFilesDir()](https://developer.android.com/reference/android/content/Context.htmlgetExternalFilesDir(java.lang.String))————申请了SD卡写权限后，你可以在SD的任何地方写数据，把你的数据写在设计好的合适位置会更加有礼貌。这样数据可以及时被清理，也会有更好的用户体验。此外，Android 4.0 Kitkat中在这个文件夹下写数据是不需要权限的，每个用户有自己的独立的数据存储路径。
译者注：该API从V8才开始支持。

[SparseArray——Map](https://developer.android.com/reference/android/util/SparseArray.html)的高效优化版本。推荐了解姐妹类SparseBooleanArray、SparseIntArray和SparseLongArray。

[PackageManager.setComponentEnabledSetting()](https://developer.android.com/reference/android/content/pm/PackageManager.htmlsetComponentEnabledSetting(android.content.ComponentName,%20int,%20int))——可以用来启动或者禁用程序清单中的组件。对于关闭不需要的功能组件是非常赞的，比如关掉一个当前不用的广播接收器。

[SQLiteDatabase.yieldIfContendedSafely()](https://developer.android.com/reference/android/database/sqlite/SQLiteDatabase.htmlyieldIfContendedSafely())——让你暂时停止一个数据库事务， 这样你可以就不会占用太多的系统资源。

[Environment.getExternalStoragePublicDirectory()](https://developer.android.com/reference/android/os/Environment.html#getExternalStoragePublicDirectory(java.lang.String))——还是那句话，用户期望在SD卡上得到统一的用户体验。用这个方法可以获得在用户设备上放置指定类型文件（音乐、图片等）的正确目录。

[View.generateViewId()](https://developer.android.com/reference/android/view/View.htmlgenerateViewId())——每次我都想要推荐动态生成控件的ID。需要注意的是，不要和已经存在的控件ID或者其他已经生成的控件ID重复。

[ActivityManager.clearApplicationUserData()](https://developer.android.com/reference/android/app/ActivityManager.htmlclearApplicationUserData())—— 一键清理你的app产生的用户数据，可能是做用户退出登录功能，有史以来最简单的方式了。

[Context.createConfigurationContext()](http://developer.android.com/reference/android/content/Context.htmlcreateConfigurationContext(android.%E2%80%94%E2%80%94ontent.res.Configuration)) ——自定义你的配置环境信息。我通常会遇到这样的问题：强制让一部分显示在某个特定的环境下（倒不是我一直这样瞎整，说来话长，你很难理解）。用这个实现起来可以稍微简单一点。

[ActivityOptions](http://developer.android.com/reference/android/app/ActivityOptions.html) ——方便的定义两个Activity切换的动画。 使用[ActivityOptionsCompat](http://developer.android.com/reference/android/support/v4/app/ActivityOptionsCompat.html) 可以很好解决旧版本的兼容问题。

[AdapterViewFlipper.fyiWillBeAdvancedByHostKThx()](http://developer.android.com/reference/android/widget/AdapterViewFlipper.htmlfyiWillBeAdvancedByHostKThx%28%29)——仅仅因为很好玩，没有其他原因。在整个安卓开源项目中（AOSP the Android ——pen Source Project Android开放源代码项目）中还有其他很有意思的东西（比如 GRAVITY_DEATH_STAR_I）。不过，都不像这个这样，这个确实有用。
译者注：该API从V11才开始支持。

[ViewParent.requestDisallowInterceptTouchEvent()](http://developer.android.com/reference/android/view/ViewParent.htmlrequestDisallowInterceptTouchEvent%28boolean%29) ——Android系统触摸事件机制大多时候能够默认处理，不过有时候你需要使用这个方法来剥夺父级控件的控制权（顺便说一下，如果你想对Android触摸机制了解更多，这个演讲会令你惊叹不已。）