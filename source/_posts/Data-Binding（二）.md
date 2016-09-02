title: Data Binding（二）
date: 2015-08-16 14:00:11
categories: Android
tags: [KVO,数据绑定,MVVM]
---
<!--more-->
## 在Fargment中使用
```xml
<layout>
    <date>
        <variable
            name="user"
            type="io.github.seniorzhai.databindingdemo.model.PlainUser" />
    </date>
</layout>
```
在Fragment.java中
```java
onCreateView() {
    ItemGridBinding binding = DataBindingUtil.inflate(inflater, R.layout.item_grid, container, false);
    PlainUser user = new PlainUser();
    user.name.set("SBBBBBBBB");
    user.age.set(111);
    binding.setUser(user);
    binding.getRoot().setBackgroundColor(RandomColor.getColor());
    return binding.getRoot();
}
```
<https://github.com/SeniorZhai/DataBindingDemo/blob/master/app/src/main/java/io/github/seniorzhai/databindingdemo/BingdingFragment.java>

## 在GridView中使用
主要的实现代码需要在Adapter的getView方法中实现
```java
public View getView(int position,View convertView,ViewGroup parent) {
    if (convertView == null) {
        binding = DataBindingUtil.inflate(inflater,R.layout.item_grid,parent,false);
        convertView = binding.getRoot();
        convertView.setTag(binding);
    } else {
        binding = convertView.getTag();
    }
    binding.setVariable(BR.user,getItem(position));
    return converView;
} 
```
<https://github.com/SeniorZhai/DataBindingDemo/blob/master/app/src/main/java/io/github/seniorzhai/databindingdemo/MyAdapter.java>

