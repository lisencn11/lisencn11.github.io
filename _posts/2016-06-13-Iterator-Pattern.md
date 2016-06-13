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

![template0](http://www.dofactory.com/images/diagrams/net/template.gif)

### 定义

**模版方法模式**在一个方法中定义一个算法的骨架，而将一些步骤延迟到子类中。模版方法使得子类可以在不改变算法结构的情况下，重新定义算法中的某些步骤。

# 好莱坞原则

**别调用我们，我们会调用你。**

在好莱坞原则之下，我们允许低层组件将自己挂钩到系统上，但是高层组件会决定什么时候喝怎样使用这些低层组件。换句话说，高层组件对待低层组件的方式是“别调用我们，我们会调用你”。

# 钩子

钩子是一种被声明在抽象类中的方法，但只有空的或者默认的实现。

钩子的存在，可以让子类有能力对算法的不同点进行挂钩。要不要挂钩，由子类自行决定。

```java

public abstract class CaffeineBeverageWithHook {
    void prepareRecipe() {
        boilWater();
        brew();
        pourInCup();

        if (customerWantsCondiments()) { // hook
            addCondiments();
        }
    }

    abstract void brew();

    abstract void addCondiments();

    void boilWater() {
        System.out.println("Boiling water");
    }

    void portInCup() {
        System.out.println("Pouring into cup");
    }

    boolean customerWantsCondiments() { // hook
        return true;
    }
}

```

加上一个条件语句，**customerWantsCondiments()**是一个钩子，有一个缺省实现，子类可以覆盖这个方法。

### 使用钩子的时间

当子类“必须”提供算法中某个方法或步骤的实现时，就使用抽象方法。

如果算法的这个部分是可选的，就用钩子。