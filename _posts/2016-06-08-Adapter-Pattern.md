---
layout: post
title: Adapter Pattern Summary
date: 2016-06-08
categories: blog
tags: [study]

---

# 设计模式-适配器模式(Adapter Pattern)

将一个接口转换成另一个接口，以符合客户的期望。

类比中标转美标的转换插头。

假设已有一个软件系统，希望和一个新的厂商类库搭配使用，但新厂商的接口不同于旧厂商接口，需要写一个类将新厂商接口转换成所期待的接口。

![Adapter Pattern](https://shishirkumarblog.files.wordpress.com/2011/07/adapter.jpg)

# 火鸡适配为鸭子

**鸭子**

```java

public interface Duck {
    public void quack();
    public void fly();
}

```

**火鸡**

```java

public interface Turkey {
    public void gobble();
    public void shortFly();
}

```

**适配器**

```java

public class TurkeyAdapter implements Duck {
    Turkey turkey;
    
    public TurkeyAdapter(Turkey turkey) {
        this.turkey = turkey;
    }
    
    public void quack() {
        turkey.gobble();
    }
    
    public void fly() {
        for (int i = 0; i < 5; i++) {
            turkey.shortFly();
        }
    }
}

```

# 适配器模式解析

![Adapter Pattern 1](https://obsoletedeveloper.files.wordpress.com/2012/09/hf-adapter.jpg)

### 客户使用适配器过程

1. 客户通过目标接口调用适配器的方法对适配器发出请求
2. 适配器使用被适配者接口把请求转换成被适配者的一个或多个调用接口。
3. 客户接受到调用的结果，但并未察觉这一切是适配器在起转换作用。

# 适配器模式定义

**适配器模式**---将一个类的接口，转换成客户期望的另一个接口。适配器让原本接口不兼容的类可以合作无间。

# 类图

![UML](http://www.dofactory.com/images/diagrams/net/adapter.gif)

* 客户只看到目标接口
* 适配器实现目标接口
* 适配器与被适配者组合
* 所有的请求都委托给被适配者

# 对象和类的适配器

之前是对象适配器的图，其实有两种适配器，下面是类适配器的图。

![UML1](http://fuzzduck.com/images/ClassAdapterUML.gif)

由于类适配器需要多重继承实现，而Java不支持多重继承，所以在Java中无法实现。

# 类似模式对比

<table>
    <tr>
        <th>装饰者模式</th>
        <td>不改变接口，但加入责任</td>
    </tr>
    <tr>
        <th>适配器模式</th>
        <td>将一个接口转成另一个接口</td>
    </tr>
    <tr>
        <th>外观模式</th>
        <td>让接口更简单</td>
    </tr>
</table>
