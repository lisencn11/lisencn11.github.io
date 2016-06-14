---
layout: post
title: Iterator Pattern Summary
date: 2016-06-12
categories: blog
tags: [study]

---

# 设计模式-迭代器模式(Iterator Pattern)

定义了一个算法的步骤，并允许子类为一个或多个步骤提供实现。

### 类图

![template0](http://pic002.cnblogs.com/images/2012/358984/2012042721423687.png)

### 定义

**迭代器模式**提供一种方法顺序访问一个聚合对象中的各个元素，而又不暴露其内部的表示。

# 单一责任原则

**一个类应该只有一个引起变化的原因**

这个原则告诉我们将一个责任只指派给一个类。例如如果不使用迭代器，那么一个类不但要完成自己的事情（管理某种聚合），孩童时要承担遍历的责任。如果有一个类具有两个改变的原因，那么会使得该类的变化几率上升。

# 组合模式

### 定义

**组合模式**允许你将对象组合成树形结构来表现“整体/部分”层次结构。组合能让客户以一致的方式处理个别对象以及对象组合。

![composite](http://blog.lukaszewski.it/wp-content/uploads/2013/11/coposite_diagram.png)

### 利用组合设计菜单

![composite](http://pic002.cnblogs.com/images/2012/358984/2012050320423780.png)