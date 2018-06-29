title: AndroidStudio快捷键汇总
date: 2015-02-05 16:46:29
categories: Android 
tags: [keymap,快捷键]
---
最近开始全面转向Android Studio开发了，经常要去查快捷键，索性汇总下，自己方便查找
<!--more-->
##IDE
|按键|说明|
|:---|:---|
|F1|帮助|
|Alt(Option)+F1|查找文件所在目录位置|
|Alt(Option)+1|快速打开或隐藏工程面板|
|Ctrl(Command)+Alt(Option)+|打开设置对话框|
|Alt(Option)+Home|跳转到导航栏|
|Esc|光标返回编辑框|
|Shift+Esc|光标返回编辑框,关闭无用的窗口|
|Shift+Click|关闭标签页|
|F12|把焦点从编辑器移到最近使用的工具窗口|
|Ctrl(Command)+Alt(Option)+Y|同步|
|Ctrl(Command)+Alt(Option)+S|打开设置对话框|
|Alt(Option)+Shift+Inert|开启/关闭列选择模式|
|Ctrl(Command)+Alt(Option)+Shift+S|打开当前项目/模块属性|
|Alt(Option)+Shift+C|查看文件的变更历史|
|Ctrl(Command)+Shift+F10|运行|
|Ctrl(Command)+Shift+F9|debug运行|
|Ctrl(Command)+Alt(Option)+F12|资源管理器打开文件夹|

##编辑
|按键|说明|
|:---|:---|
|Ctrl(Command)+C|复制当前行或选中的内容|
|Ctrl(Command)+D|粘贴当前行或选中的内容|
|Ctrl(Command)+X|剪切当前行或选中的内容|
|Ctrl(Command)+Y|删除行|
|Ctrl(Command)+Z|倒退|
|Ctrl(Command)+Shift+Z|向前|
|Alt(Option)+Enter|自动修正|
|Ctrl(Command)+Alt(Option)+L|格式化代码|
|Ctrl(Command)+Alt(Option)+I|将选中的代码进行自动缩进编排|
|Ctrl(Command)+Alt(Option)+O|优化导入的类和包|
|Alt(Option)+Insert|得到一些Intention Action，可以生成构造器、Getter、Setter、将 <code>==</code> 改为 <code>equals()</code> 等|
|Ctrl(Command)+Shift+V|选最近使用的剪贴板内容并插入|
|Ctrl(Command)+Alt(Option)+Shift+V|简单粘贴|
|Ctrl(Command)+Shift+Insert|选最近使用的剪贴板内容并插入（同Ctrl(Command)+Shift+V）|
|Ctrl(Command)+Enter|在当前行的上面插入新行，并移动光标到新行（此功能光标在行首时有效）|
|Shift+Enter|在当前行的下面插入新行，并移动光标到新行|
|Ctrl(Command)+J|自动代码|
|Ctrl(Command)+Alt(Option)+T|把选中的代码放在 <code>try{}</code> 、<code>if{}</code> 、 <code>else{}</code> 里|
|Shift+Alt(Option)+Insert|竖编辑模式|
|Ctrl(Command)+ <code>/</code>|注释 <code>//</code> |
|Ctrl(Command)+Shift+ <code>/</code>|注释 <code>/*...*/</code>|
|Ctrl(Command)+Shift+J|合并成一行|
|F2/Shift+F2|跳转到下/上一个错误语句处|
|Ctrl(Command)+Shift+Back|跳转到上次编辑的地方|
|Ctrl(Command)+Alt(Option)+Space|类名自动完成|
|Shift+Alt(Option)+Up/Down|内容向上/下移动|
|Ctrl(Command)+Shift+Up/Down|语句向上/下移动|
|Ctrl(Command)+Shift+U|大小写切换|
|Tab|代码标签输入完成后，按 <code>Tab</code>，生成代码|
|Ctrl(Command)+Backspace|按单词删除|
|Ctrl(Command)+Shift+Enter|语句完成|
|Ctrl(Command)+Alt(Option)+J|用动态模板环绕|
##文件
|按键|说明|
|:---|:---|
|Ctrl(Command)+F12|显示当前文件的结构|
|Ctrl(Command)+H|显示类继承结构图|
|Ctrl(Command)+Q|显示注释文档|
|Ctrl(Command)+P|方法参数提示|
|Ctrl(Command)+U|打开当前类的父类或者实现的接口|
|Alt(Option)+Left/Right|切换代码视图|
|Ctrl(Command)+Alt(Option)+Left/Right|返回上次编辑的位置|
|Alt(Option)+Up/Down|在方法间快速移动定位|
|Ctrl(Command)+B|快速打开光标处的类或方法|
|Ctrl(Command)+W|选中代码，连续按会有其他效果|
|Ctrl(Command)+Shift+W|取消选择光标所在词|
|Ctrl(Command)+ <code>-</code> / <code>+</code>|折叠/展开代码|
|Ctrl(Command)+Shift+ <code>-</code> / <code>+</code>|折叠/展开全部代码|
|Ctrl(Command)+Shift+<code>.</code>|折叠/展开当前花括号中的代码|
|Ctrl(Command)+ <code>]</code> / <code>[</code>|跳转到代码块结束/开始处|
|F2 或 Shift+F2|高亮错误或警告快速定位|
|Ctrl(Command)+Shift+C|复制路径|
|Ctrl(Command)+Alt(Option)+Shift+C|复制引用，必须选择类名|
|Alt(Option)+Up/Down|在方法间快速移动定位|
|Shift+F1|要打开编辑器光标字符处使用的类或者方法 <code>Java</code> 文档的浏览器|
|Ctrl(Command)+G|定位行|

##查找
|按键|说明|
|:---|:---|
|Ctrl(Command)+F|在当前窗口查找文本|
|Ctrl(Command)+Shift+F|在指定环境下查找文本|
|F3|向下查找关键字出现位置|
|Shift+F3|向上一个关键字出现位置|
|Ctrl(Command)+R|在当前窗口替换文本|
|Ctrl(Command)+Shift+R|在指定窗口替换文本|
|Ctrl(Command)+N|查找类|
|Ctrl(Command)+Shift+N|查找文件|
|Ctrl(Command)+Shift+Alt(Option)+N|查找项目中的方法或变量|
|Ctrl(Command)+B|查找变量的来源|
|Ctrl(Command)+Alt(Option)+B|快速打开光标处的类或方法|
|Ctrl(Command)+Shift+B|跳转到类或方法实现处|
|Ctrl(Command)+E|最近打开的文件|
|Alt(Option)+F3|快速查找，效果和Ctrl(Command)+F相同|
|F4|跳转至定义变量的位置|
|Alt(Option)+F7|查询当前元素在工程中的引用|
|Ctrl(Command)+F7|查询当前元素在当前文件中的引用，然后按 <code>F3</code> 可以选择|
|Ctrl(Command)+Alt(Option)+F7|选中查询当前元素在工程中的引用|
|Ctrl(Command)+Shift+F7|高亮显示匹配的字符，按 <code>Esc</code> 高亮消失|
|Ctrl(Command)+Alt(Option)+F7|查找某个方法的所有调用地方|
|Ctrl(Command)+Shift+Alt(Option)+N|查找类中的方法或变量|
|<em>Ctrl(Command)+Shift+O</em>|<em>弹出显示查找内容</em>|
|<em>Ctrl(Command)+Alt(Option)+Up/Down</em>|<em>快速跳转搜索结果</em>|
|<em>Ctrl(Command)+Shift+S</em>|<em>高级搜索、搜索结构</em>|

##重构
|按键|说明|
|:---|:---|
|F5|复制|
|F6|移动|
|Alt(Option)+Delete|安全删除|
|Ctrl(Command)+U|转到父类|
|Ctrl(Command)+O|重写父类的方法|
|Ctrl(Command)+I|实现方法|
|Ctrl(Command)+Alt(Option)+N|内联|
|Ctrl(Command)+Alt(Option)+Shift+T|弹出重构菜单|
|Shift+F6|重构-重命名|
|Ctrl(Command)+Alt(Option)+M|提取代码组成方法|
|Ctrl(Command)+Alt(Option)+C|将变量更改为常量|
|Ctrl(Command)+Alt(Option)+V|定义变量引用当前对象或者方法的返回值|
|Ctrl(Command)+Alt(Option)+F|将局部变量更改为类的成员变量|
|Ctrl(Command)+Alt(Option)+P|将变量更改为方法的参数|

##调试
|按键|说明|
|:---|:---|
|F8|跳到下一步|
|Shift+F8|跳出函数、跳到下一个断点|
|Alt(Option)+Shift+F8|强制跳出函数|
|F7|进入代码|
|Shift+F7|智能进入代码|
|Alt(Option)+Shift+F7|强制进入代码|
|Alt(Option)+F9|运行至光标处|
|Ctrl(Command)+Alt(Option)+F9|强制运行至光标处|
|Ctrl(Command)+F2|停止运行|
|Alt(Option)+F8|计算变量值|

##VCS
|按键|说明|
|Alt(Option)+ <code>~</code>|
|<code>VCS</code> 操作菜单|
|Ctrl(Command)+K|提交更改|
|Ctrl(Command)+T|更新项目|
|Ctrl(Command)+Alt(Option)+Shift+D|显示变化|