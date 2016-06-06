---
layout: post
title: Decorator Pattern Summary
date: 2016-06-03
categories: blog
tags: [study]

---

# 设计模式-装饰者模式

用对象组合的方式，做到在运行时装饰类。能够在不修改任何底层代码的情况下，给对象赋予新的职责。


# 设计原则

开放-关闭原则

>类应该对扩展开放，对修改关闭。

# 定义

装饰者模式动态地将责任附加到对象上。若要扩展功能，装饰者提供了比继承更有弹性的替代方案。

# 缺点

* 大量的小类
* 有些代码会依赖特定的类型，这样的代码导入装饰者会出问题
* 实例化组件时增加代码复杂度

# 代码

```java

public abstract class Beverage {
    String description = "Unknown Beverage";

    public String getDescription() {
        return description;
    }

    public abstract double cost();
}

public class Espresso extends Beverage {
    public Espresso() {
        description = "Espresso";
    }

    public double cost() {
        return 1.99;
    }
}

public class HouseBlend extends Beverage {
    public HouseBlend() {
        description = "House Blend Coffee";
    }

    public double cost() {
        return 0.89;
    }
}

public abstract class CondimentDecorator extends Beverage {
    public abstract String getDescription();
}

public class Mocha extends CondimentDecorator {
    Beverage beverage;

    public Mocha(Beverage beverage) {
        this.beverage = beverage;
    }

    public String getDescription() {
        return beverage.getDescription() + ", Mocha";
    }

    public double cost() {
        return 0.2 + beverage.cost();
    }
}

public class Soy extends CondimentDecorator {
    Beverage beverage;

    public Soy(Beverage beverage) {
        this.beverage = beverage;
    }

    public String getDescription() {
        return beverage.getDescription() + ", Soy";
    }

    public double cost() {
        return 0.15 + beverage.cost();
    }
}

public class Whip extends CondimentDecorator {
    Beverage beverage;

    public Whip(Beverage beverage) {
        this.beverage = beverage;
    }

    public String getDescription() {
        return beverage.getDescription() + ", Whip";
    }

    public double cost() {
        return 0.1 + beverage.cost();
    }
}

import java.io.BufferedInputStream;
import java.io.FileInputStream;
import java.io.InputStream;
import java.io.IOException;

public class Test {
    public static void main(String[] args) {
        // Beverage beverage = new Espresso();
        // System.out.println(beverage.getDescription() + " $" + beverage.cost());

        // Beverage beverage2 = new HouseBlend();
        // beverage2 = new Mocha(beverage2);
        // beverage2 = new Mocha(beverage2);
        // beverage2 = new Whip(beverage2);
        // System.out.println(beverage2.getDescription() + " $" + beverage2.cost());

        // Beverage beverage3 = new Espresso();
        // beverage3 = new Soy(beverage3);
        // beverage3 = new Mocha(beverage3);
        // beverage3 = new Whip(beverage3);
        // System.out.println(beverage3.getDescription() + " $" + beverage3.cost());
    }
}

```

