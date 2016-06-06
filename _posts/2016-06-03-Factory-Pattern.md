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

### 引入工厂模式

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
*具体创建者*---能够产生产品的类称为具体创建者。如下，每个加盟店有自己的PizzaStore子类，可以利用实现createPizza()创建自己风味的比萨:

```java

public class NYPizzaStore extends PizzaStore {
    protected Pizza createPizza(String item) {
        if (item.equals("cheese")) {
            return new NYStyleCheesePizza();
        } //else if (item.equals("veggie")) {
        //     return new NYStyleVeggiePizza();
        // } else if (item.equals("clam")) {
        //     return new NYStyleClamPizza();
        // } else if (item.equals("pepperoni")) {
        //     return new NYStylePepperoniPizza();
        // } else return null;
        else {
            return null;
        }
    }
}

```

##### 产品类

*抽象产品*---抽象工厂生产产品，对PizzaStore来说，产品就是Pizza。

```java

public abstract class Pizza {
    String name;
    String dough;
    String sauce;
    ArrayList<String> toppings = new ArrayList<String>();

    void prepare() {
        System.out.println("Preparing " + name);
        System.out.println("Tossing dough...");
        System.out.println("Adding sauce...");
        System.out.println("Adding toppings");
        for (int i = 0; i < toppings.size(); i++) {
            System.out.println("    " + toppings.get(i));
        }
    }

    void cut() {
        System.out.println("Cutting the pizza into diagonal slices");
    }

    void box() {
        System.out.println("Place pizza in official PizzaStore box");
    }

    void bake() {
        System.out.println("Bake for 25 minutes at 350");
    }

    public String getName() {
        return name;
    }
}

```

*具体产品*---所有店里能实际制造的比萨。

```java

public class NYStyleCheesePizza extends Pizza {
    public NYStyleCheesePizza() {
        name = "NY style Sauce and Cheese Pizza";
        dough = "Thin Crust Dough";
        sauce = "Marinara Sauce";

        toppings.add("Grated Reggiano Cheese");
    }
}

```

### 工厂模式定义

*工厂方法模式*---定义了一个创建对象的接口，但由子类决定要实例化的类是哪一个。工厂方法让类把实例化推迟到子类。

编写创建者类时，不需要知道实际创建的产品是哪一个，选择了使用哪个子类，自然就决定了实际创建的产品是什么。

### 工厂模式好处

将创建对象的代码集中在一个对象或方法中，可以避免代码中的重复，并且更方便以后的维护。

可以帮助针对接口编程，而不针对实现变成。

# 对象依赖

当你*直接实例化*一个对象时，就是在依赖它的具体类。如果不使用工厂模式，PizzaStore代码如下:

```java

public class DependentPizzaStore {
    public Pizza createPizza(String style, String type) {
        Pizza pizza = null;
        if (style.equals("NY")) {
            if (type.equals("cheese")) {
                pizza = new NYStyleCheesePizza();
            } else if (type.equals("veggie")) {

            }
            // ...
        } else if (style.euqals("Chicago")) {
            if (type.equals("cheese")) {

            } else if (type.equals("veggie")) {

            }
            // ...
        }
        // ...
    }
}

```

这个PizzaStore依赖于所有的比萨对象，这些比萨具体实现的任何改变都会影响到PizzaStore。每新增一个比萨种类，就等于让PizzaStore多了一个依赖。

# 依赖倒置原则

要依赖抽象，不要依赖具体类。

解释：PizzaStore是“高层组件”，比萨实现是“低层组件”，而PizzaStore依赖这些具体比萨类。这个原则告诉我们，应该重写代码以便于我们依赖抽象类，而不依赖具体类。

# 原则的应用

应用工厂方法，PizzaStore依赖Pizza这个抽象类，具体比萨类，也依赖Pizza抽象。由于具体比萨类也依赖Pizza抽象，与一般OO设计思考方式相反，所以称为“倒置”。

##### 指导方针

* 变量不可以持有具体类的引用
* 不要让类派生自具体类
* 不要覆盖基类中已实现的方法

# 抽象工厂模式

*抽象工厂模式*---提供一个接口，用于创建相关或依赖对象的家族，而不需要明确指定具体类。

# 工厂方法vs抽象工厂

* 工厂方法和抽象工厂都是负责创建对象
* 工厂方法使用继承，而抽象工厂使用对象的组合
* 工厂方法: 通过子类来创建对象
* 抽象工厂: 提供一个用来创建一个产品家族的抽象类型，这个类型的子类定义了产品被产生的方法
* 