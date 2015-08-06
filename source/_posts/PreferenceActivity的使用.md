title: PreferenceActivity的使用
date: 2015-08-04 18:56:37
categories: Android
tags: [PerferenceScreencreen]
---
<!--more-->
1. PreferenceScreen 设置页面，可嵌套二级设置页面
2. PreferenceCategory 设置类，可用Title参数设置标题
3. CheckBoxPreference 可选设置，可用Title设置标题，用summaryOn和summaryOff参数设置控件选中和未选中时的提示，可用defaultValue设置缺省值
4. ListPreference 下拉框选择设置，可用Title设置标题，用Summary参数设置说明，点击出现下拉框，用dialogTitle设置下拉框的标题，entries和entryValues分别表示显示的值和代码中获取的真正的值
5. EditTextPreference 输入框设置，点击输入字符串设置，用Title参数设置标题，Summary参数设置说明，dialogTitle参数设置输入框的标题
6. RingtonePreference 铃声选择设置，点击后可选择系统铃声，用Title参数设置标题，Summary参数设置说明，dialogTitle参数设置铃声选择的标题
