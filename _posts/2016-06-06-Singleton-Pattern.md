---
layout: post
title: Singleton Pattern Summary
date: 2016-06-06
categories: blog
tags: [study]

---

# 设计模式-单件模式

### 用途

用来创建只能有一个实例的对象。

注册表配置信息：不能有多份拷贝，会把设置搞得一团糟。

管理共享的资源：数据库连接或者线程池。

### 全局变量的缺点

程序一开始就需要创建，但却未必用到，浪费了资源。

### 单件模式经典实现

```java

public class Singleton {
    private static Singleton uniqueInstance;

    private Singleton() {}

    public static Singleton getInstance() {
        if (uniqueInstance == null) {
            uniqueInstance = new Singleton();
        }
        return uniqueInstance;
    }
}

```

### 单件模式定义

*单件模式*---确保一个类只有一个实例，并提供一个全局访问点。

### 多线程问题

多线程时存在产生多个对象的可能性。

```java

public static ChocolateBoiler getInstance() {
    if (uniqueInstance == null) {
        uniqueInstance = new ChocolateBoiler();
    }
    return uniqueInstance;
}

```
两个线程先后进入if后，第一个线程先new，并return，然后第二个线程再new并return，会造成两个线程有两个不同的对象。

##### 解决方案

synchronized关键字，能够保证同一时间只有一个线程进入这个方法。

```java

public static synchronized ChocolateBoiler getInstance() {
    if (uniqueInstance == null) {
        uniqueInstance = new ChocolateBoiler();
    }
    return uniqueInstance;
}

```
解决问题的同时导致效率低下，因为只有第一次执行此方法时才需要同步。

##### 改善多线程

* 如果getInstance()方法执行次数不多，可以接受额外开销
* 直接用静态初始化器创建单件：private static Singleton uniqueInstance = new Singleton()，这样JVM在加载这个类时会马上创建此唯一的单件实例。
* 用“双重检查加锁”(double-checked locking)，只有在实例没有创建的时候才同步。

```java

private volatile static Singleton uniqueInstance;

public static ChocolateBoiler getInstance() {
    if (uniqueInstance == null) {
        synchronized (Singleton.class) {
            if (uniqueInstance == null) {
                uniqueInstance = new ChocolateBoiler();
            }
        }
    }
    return uniqueInstance;
}

```