title: Foundation框架(二)
date: 2014-03-04 12:49:18
categories: iOS
tags: [iOS]
---
## Objective-C集合
- 大致可分为：NSArray、NSSet、NSDictionary三种体系，NSAarray代表有序、可重复的集合，NSSet代表无序、不可重复的集合，NSDictionary代表具有映射关系的集合。
## 数组（NSArray和NSMutableArray）
- NSArray代表元素有序、可重复的一个集合，集合中每一个元素都有对应的顺序索引。
-常用方法
	+ array:创建一个不包含任何元素的空NSArray
	+ arrayWithContentsOfFile:/initWithContentsOfFile:读取文件内容来创建NSArray
	+ arrayWithObject:/initWithObject:创建只包含指定元素的NSArray
	+ arrayWithObjects:/initWithObjects:创建包含指定N个元素的NSArray
	+ objectAtIndex:根据索引返回元素
	+ lastObject:最后一个元素
	+ objectsAtIndexes:[NSIndexSet indexSetWithIndexesInRnnge:NSMakeRange(2,3)] 从索引中2~5的元素组成新集合
	+ indexOfObject:查找元素的位置
	+ indexOfObject: inRange:查找指定范围内元素的位置
	+ arrayByAddingObject: 追加元素
	+ arrayByAddingObjectsFormArray:追加数组集合
	+ writeToFile:写入文件