---
layout: post
title: State Pattern Summary
date: 2016-06-16
categories: blog
tags: [study]

---

# 设计模式-状态模式(State Pattern)

### 通用的技巧

如何对对象内的状态建模----通过创建一个实例变量来持有状态值，并在方法内书写条件代码来处理不同状态。

### 状态机类图

![state_machine_0](https://www.safaribooksonline.com/library/view/head-first-design/0596007124/httpatomoreillycomsourceoreillyimages1420580.png.jpg)

### 状态机代码

```java
public class GumballMachine {
    final static int SOLD_OUT = 0;
    final static int NO_QUARTER = 1;
    final static int HAS_QUARTER = 2;
    final static int SOLD = 3;

    int state = SOLD_OUT;
    int count = 0;

    public GumballMachine(int count) {
        this.count = count;
        if (count > 0) {
            state = NO_QUARTER;
        }
    }

    public void insertQuarter() {
        if (state == HAS_QUARTER) {
            System.out.println("You can't insert another quarter.");
        } else if (state == SOLD_OUT) {
            System.out.println("You can't insert a quarter, the machine is sold out.");
        } else if (state == SOLD) {
            System.out.println("Please wait, we're already gicing you a gumball.");
        } else if (state == NO_QUARTER) {
            state = HAS_QUARTER;
            System.out.println("You inserted a quarter.");
        }
    }

    public void ejectQuarter() {
        if (state == HAS_QUARTER) {
            System.out.println("Quarter returned.");
            state = NO_QUARTER;
        } else if (state == NO_QUARTER) {
            System.out.println("You haven't inserted a quarter.");
        } else if (state == SOLD) {
            System.out.println("Sorry, you already turned the crank.");
        } else if (state == SOLD_OUT) {
            System.out.println("You can't eject, you haven't inserted a quarter yet.");
        }
    }

    public void turnCrank() {
        if (state == SOLD) {
            System.out.println("Turning twice doesn't get you another gumball.");
        } else if (state == NO_QUARTER) {
            System.out.println("You turned but there's no quarter.");
        } else if (state == SOLD_OUT) {
            System.out.println("You turned, but there are no quarter.");
        } else if (state == HAS_QUARTER) {
            System.out.println("You turned...");
            state = SOLD;
            dispense();
        }
    }

    public void dispense() {
        if (state == SOLD) {
            System.out.println("A gumball comes rolling out the slot.");
            count = count - 1;
            if (count == 0) {
                System.out.println("Oops, out of gumball.");
                state = SOLD_OUT;
            } else {
                state = NO_QUARTER;
            }
        } else if (state == NO_QUARTER) {
            System.out.println("You need to pay first.");
        } else if (state == SOLD_OUT) {
            System.out.println("No gumball dispensed.");
        } else if (state == HAS_QUARTER) {
            System.out.println("No gumball dispensed.");
        }
    }

    public String toString() {
        StringBuffer sb = new StringBuffer();
        sb.append("Machine state: ");
        if (state == SOLD_OUT) {
            sb.append("SOLD OUT.");
        } else if (state == NO_QUARTER) {
            sb.append("NO QUARTER.");
        } else if (state == HAS_QUARTER) {
            sb.append("HAS QUARTER.");
        } else if (state == SOLD) {
            sb.append("SOLD.");
        }
        sb.append(" Gumball left: " + count + ".");
        return sb.toString();
    }

    public void refill(int count) {
        if (state == SOLD_OUT) {
            state = NO_QUARTER;
        } else if (state == NO_QUARTER) {
            state = NO_QUARTER;
        } else if (state == HAS_QUARTER) {
            state = HAS_QUARTER;
        } else if (state == SOLD) {
            state = SOLD;
        }
        this.count += count;
        System.out.println("Gumball refilled by " + count + ".");
    }
}


public class Test {
    public static void main(String[] args) {
        GumballMachine gumballMachine = new GumballMachine(5);

        System.out.println(gumballMachine);

        gumballMachine.insertQuarter();
        gumballMachine.turnCrank();

        System.out.println(gumballMachine);

        gumballMachine.insertQuarter();
        gumballMachine.ejectQuarter();
        gumballMachine.turnCrank();

        System.out.println(gumballMachine);

        gumballMachine.insertQuarter();
        gumballMachine.turnCrank();
        gumballMachine.insertQuarter();
        gumballMachine.turnCrank();
        gumballMachine.ejectQuarter();

        System.out.println(gumballMachine);

        gumballMachine.insertQuarter();
        gumballMachine.insertQuarter();
        gumballMachine.turnCrank();
        gumballMachine.insertQuarter();
        gumballMachine.turnCrank();
        gumballMachine.insertQuarter();
        gumballMachine.turnCrank();

        System.out.println(gumballMachine);

        gumballMachine.refill(5);

        System.out.println(gumballMachine);
    }
}
```

# 新的设计

重写代码，以便于将**状态对象**封装在各自的类中，然后在动作发生时委托给当前状态。

1. 定义一个State接口，在这个接口内，糖果机的每个动作都有一个对应的方法。
2. 然后为机器中的每个状态实现状态类，这些类负责在对应状态下进行机器的行为。
3. 摆脱旧的条件代码，取而代之，将动作委托到状态类。

### 状态模式代码

```java
public interface State {
    public void insertQuarter();
    public void ejectQuarter();
    public void turnCrank();
    public void dispense();
}


public class NoQuarterState implements State {
    NewGumballMachine gumballMachine;

    public NoQuarterState(NewGumballMachine gumballMachine) {
        this.gumballMachine = gumballMachine;
    }

    public void insertQuarter() {
         System.out.println("You inserted a quarter.");
         gumballMachine.setState(gumballMachine.getHasQuarterState());
    }

    public void ejectQuarter() {
        System.out.println("You haven't inserted a quarter.");
    }

    public void turnCrank() {
        System.out.println("You turned but there's no quarter.");
    }

    public void dispense() {
        System.out.println("You need to pay first.");
    }
}


public class HasQuarterState implements State {
    NewGumballMachine gumballMachine;

    public HasQuarterState(NewGumballMachine gumballMachine) {
        this.gumballMachine = gumballMachine;
    }

    public void insertQuarter() {
        System.out.println("You can't insert another quarter.");
    }

    public void ejectQuarter() {
        System.out.println("Quarter returned.");
        gumballMachine.setState(gumballMachine.getNoQuarterState());
    }

    public void turnCrank() {
        System.out.println("You turned...");
        gumballMachine.setState(gumballMachine.getSoldState());
        dispense();
    }

    public void dispense() {
        System.out.println("No gumball dispensed.");
    }
}


public class SoldState implements State {
    NewGumballMachine gumballMachine;

    public SoldState(NewGumballMachine gumballMachine) {
        this.gumballMachine = gumballMachine;
    }

    public void insertQuarter() {
        System.out.println("Please wait, we're already gicing you a gumball.");
    }

    public void ejectQuarter() {
        System.out.println("Sorry, you already turned the crank.");
    }

    public void turnCrank() {
        System.out.println("Turning twice doesn't get you another gumball.");
    }

    public void dispense() {
        gumballMachine.releaseBall();
        if (gumballMachine.getCount() == 0) {
            System.out.println("Oops, out of gumball.");
           gumballMachine.setState(gumballMachine.getSoldOutState());
        } else {
            gumballMachine.setState(gumballMachine.getNoQuarterState());
        }
    }
}


public class SoldOutState implements State {
    NewGumballMachine gumballMachine;

    public SoldOutState(NewGumballMachine gumballMachine) {
        this.gumballMachine = gumballMachine;
    }

    public void insertQuarter() {
        System.out.println("You can't insert a quarter, the machine is sold out.");
    }

    public void ejectQuarter() {
        System.out.println("You can't eject, you haven't inserted a quarter yet.");
    }

    public void turnCrank() {
        System.out.println("You turned, but there are no quarter.");
    }

    public void dispense() {
        System.out.println("No gumball dispensed.");
    }
}


public class NewGumballMachine {
    State soldOutState;
    State noQuarterState;
    State hasQuarterState;
    State soldState;

    State state = soldOutState;
    int count = 0;

    public NewGumballMachine(int numberGumballs) {
        soldOutState = new SoldOutState(this);
        noQuarterState = new NoQuarterState(this);
        hasQuarterState = new HasQuarterState(this);
        soldState = new SoldState(this);
        this.count = numberGumballs;
        if (numberGumballs > 0) {
            state = noQuarterState;
        }
    }

    public void insertQuarter() {
        state.insertQuarter();
    }

    public void ejectQuarter() {
        state.ejectQuarter();
    }

    public void turnCrank() {
        state.turnCrank();
        state.dispense();
    }

    void setState(State state) {
        this.state = state;
    }

    void releaseBall() {
        System.out.println("A gumball comes rolling out the slot...");
        if (count != 0) {
            count = count - 1;
        }
    }

    State getHasQuarterState() {
        return hasQuarterState;
    }

    State getNoQuarterState() {
        return noQuarterState;
    }

    State getSoldState() {
        return soldState;
    }

    State getSoldOutState() {
        return soldOutState;
    }

    public int getCount() {
        return count;
    }
}
```

### 状态模式定义

**状态模式**允许对象在内部状态改变时改变它的行为，对象看起来好像修改了它的类。

因为模式将状态封装成独立的类，并将动作委托到代表当前状态的对象，所以行为会随着内部状态而改变。

### 状态模式类图

![State_Pattern_0](http://pic002.cnblogs.com/images/2012/358984/2012050411054690.png)

### 和策略模式类图一样

两个模式差别在于“意图”。

**策略模式**是除了继承之外的一种弹性替代方案。如果使用继承定义一个类的行为，你会被这个行为困住，有了策略模式，可以通过组合不同的对象来改变行为。

**状态模式**是不用在Context中放置许多条件判断的替代方案。通过将行为封装进状态对象中，你可以通过在Context内简单改变状态来改变Context的行为。

### 加入WinnerState

```java
public class WinnerState implements State {
    NewGumballMachine gumballMachine;

    public WinnerState(NewGumballMachine gumballMachine) {
        this.gumballMachine = gumballMachine;
    }

    public void insertQuarter() {
        System.out.println("Please wait, we're already giving you a gumball.");
    }

    public void ejectQuarter() {
        System.out.println("Sorry, you already turned the crank.");
    }

    public void turnCrank() {
        System.out.println("Turning twice doesn't get you another gumball.");
    }

    public void dispense() {
        System.out.println("YOU'RE A WINNER! You get two gumballs for your quarter");
        gumballMachine.releaseBall();
        if (gumballMachine.getCount() == 0) {
            gumballMachine.setState(gumballMachine.getSoldOutState());
        } else {
            gumballMachine.releaseBall();
            if (gumballMachine.getCount() > 0) {
                gumballMachine.setState(gumballMachine.getNoQuarterState());
            } else {
                System.out.println("Oops, out of gumballs");
                gumballMachine.setState(gumballMachine.getSoldOutState());
            }
        }
    }
}
```