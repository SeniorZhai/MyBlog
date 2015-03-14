title: TimingLogger
date: 2015-03-11 20:44:31
categories: Android
tags: [Log,用时]
---
妈妈再也不用担心我分析用时时间了
<!--more-->
```java
TimingLogger timings = new TimingLogger(TAG,"methodA");
// ...
timings.addSplit("work A");
// ...
timings.addSplit("work B");
timings.dumpToLog();
```
当调用`dumpToLog`方法时会打印Log
```
  D/TAG     ( 3459): methodA: begin
  D/TAG     ( 3459): methodA:      9 ms, work A
  D/TAG     ( 3459): methodA:      1 ms, work B
  D/TAG     ( 3459): methodA: end, 16 ms
```