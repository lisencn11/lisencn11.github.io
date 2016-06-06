---
layout: post
title: Factory Pattern Summary
date: 2016-06-03
categories: blog
tags: [study]

---

# 设计模式-工厂模式

## 存在的问题

```java

Duck duck = new MallardDuck();

```

Duck接口让代码具有弹性，但是还是得建立具体类的实例。

如：

```java

Duck duck;

if (picnic) {
    duck = new MallardDuck();
} else if (hunting) {
    duck = new DecoyDuck();
} else if (inBathTub) {
    duck = new RubberDuck();
}

```

需要在运行时根据条件来决定要实例化的类，一旦有变化或者扩展，就必须重新对代码进行检查和修改。

## 工厂模式

将创建对象的代码移到一个另一个对象，这个对象只负责如何创建Pizza，我们称这个新对象为"工厂"。

如：

```java

public class SimplePizzaFactory {
    public Pizza createPizza(String type) {
        Pizza pizza = null;

        if (type.equals("cheese")) {
            pizza = new CheesePizza();
        } else if (type.equals("pepperoni")) {
            pizza = new PepperoniPizza();
        } else if (type.equals("clam")) {
            pizza = new ClamPizza();
        } else if (type.equals("veggie")) {
            pizza = new VeggiePizza();
        }

        return pizza;
    }
}

```

所有工厂模式都用来封装对象的创建。工厂方法模式通过让子类决定该创建的对象是什么，来达到将对象创建的过程封装的目的

### 组成元素

##### 创建者类

*抽象创建者类*---定义了一个抽象的工厂方法，让子类实现此方法制造产品。创建者通常会包含依赖于抽象产品的代码，而这些抽象产品由子类制造。创建者不需要真的知道在制造哪种具体产品。如下:

```java

public abstract class PizzaStore {
    public Pizza orderPizza(String type) {
        Pizza pizza;

        pizza = createPizza(type);

        pizza.prepare();
        pizza.bake();
        pizza.cut();
        pizza.box();

        return pizza;
    }

    protected abstract Pizza createPizza(String type);
}

```
