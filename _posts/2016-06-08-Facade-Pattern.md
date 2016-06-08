---
layout: post
title: Facade Pattern Summary
date: 2016-06-08
categories: blog
tags: [study]

---

# 设计模式-外观模式(Facade Pattern)

通过实现一个提供更合理的接口的外观类，将一个复杂的子系统变得容易使用。

![Facade Pattern](http://blog.lukaszewski.it/wp-content/uploads/2013/08/home_theater_facade.png)

# 常见疑问

<table>
    <tr>
        <th>client能否与低层功能接触?</th>
        <td>可以，外观没有封装，只是提供简化的接口</td>
    </tr>
    <tr>
        <th>外观模式其他优点?</th>
        <td>将客户从组件的子系统中解耦，更换家庭影院不需要改变客户代码</td>
    </tr>
    <tr>
        <th>适配器与外观的区别在于包装的类的数量？</th>
        <td>不对！适配器也可以包装多个类。区别在于意图，适配器意图改变接口符合Client期望，外观意图提供子系统的简化接口</td>
    </tr>
</table>

# 外观模式定义

**外观模式**---提供了一个统一的接口，用来访问子系统中的一群接口。外观定义了一个高层接口，让子系统更容易使用。

![Facade Pattern UML](http://www.websitedesignwebsitedevelopment.com/wp-content/uploads/2011/06/facade7.jpg)

# OO原则-最少知识原则

**最少知识原则**: 只和你的密友谈话（减少对象之间的交互，只留下几个“密友”）

这个原则希望我们在设计中，不要让太多的类耦合在一起，免得修改系统中一部分，会影响到其他部分。

### 原则方针

在对象的方法内，我们只应该调用属于一下范围的方法：

* 该对象本身
* 被当作方法的参数而传递进来的对象
* 此方法所创建或实例化的任何对象
* 对象的任何组件

这些方针告诉我们，如果某对象是调用其他的方法的返回结果，不要调用该对象的方法。

组件可以看成被实例变量所引用的任何对象。