---
layout: post
title: Proxy Pattern Summary
date: 2016-06-20
categories: blog
tags: [study]

---

# 设计模式-代理模式(Proxy Pattern)

### 代理

控制和管理访问

* **远程代理**好比“远程对象的本地代表”。
* **远程对象**是一种对象，活在不同的JVM堆中，无法取得另一个堆的对象的引用。
* **本地代表**可以由本地方法调用的对象，行为会转发到远程对象中。

![proxy_0](http://66.media.tumblr.com/93312fa1aaa10ef5000a4c4125f86138/tumblr_inline_o17ulxFAVW1tb3m1d_500.png)

**RMI**可以让我们找到远程JVM内的对象，并允许我们调用它们的方法。

# RMI

![proxy_1](https://www.safaribooksonline.com/library/view/head-first-servlets/9780596516680/httpatomoreillycomsourceoreillyimages1382676.png.jpg)

RMI提供了客户辅助对象(RMI stub)和服务辅助对象(RMI skeleton)，为客户辅助对象创建和服务对象相同的方法。RMI的好处在于你不必亲自写任何网络或I/O代码。客户程序调用远程方法就和在运行在客户自己的本地JVM上对对象进行正常方法调用一样。

# 制作远程服务

1. **制作远程接口**：远程接口定义出可以让客户远程调用的方法。客户用它作为服务的类类型。Stub和实际的服务都实现此接口。
2. **制作远程的实现**：这是实际工作的类，为远程接口中定义的远程方法提供了真正的实现。
3. **利用rmic产生的stub和skeleton**：这是客户和服务的辅助类，不需要创建，自动处理。
4. **启动RMI registry**：rmiregistry好比电话薄，客户可以从中查到代理的位置。
5. **开始远程服务**：让服务对象开始运行。

### 步骤一：制作远程接口

##### 1. 扩展(extends)java.rmi.Remote

```java
public interface MyRemote extends Remote
```

##### 2. 声明所有的方法都会抛出RemoteException

客户使用远程接口调用服务。因为用到了网络和I/O，需要处理或声明远程异常来解决。

```java
import java.rmi.*

public interface MyRemote extends Remote {
    public String sayHello() throws RemoteException;
}
```

##### 3. 确定变量和返回值是属于原语(primitive)类型或者可序列化(Serializable)类型

远程方法的变量和返回值，必须属于原语类型或Serializable类型。原因是打包通过网络运送，自己定义的类必须实现Serializable。(String类实现了Serializable)

### 步骤二：制作远程实现

##### 1. 实现远程接口

服务必须实现远程接口

```java
public class MyRemoteImpl extends UnicastRemoteObject implements MyRemote {
    public String sayHello() {
        return "Server says, 'Hey'";
    }
}
```

##### 2. 扩展UnicastRemoteObject

为了成为远程服务对象，需要某些“远程的”功能，最简单的方式是扩展java.rmi.server.UnicastRemoteObject，让超类做这些工作。

##### 3. 设计一个不带变量的构造器，并声明RemoteException

因为UnicastRemoteObject的构造器刨出这样一个异常，我们的远程实现也需要声明一个构造器。

```java
public MyRemoteImpl() throws RemoteException { }
```

##### 4. 用RMI registry注册此服务

实例化服务，在RMI registry中注册远程服务，让它可以被远程客户调用。

```java
try {
    MyRemote service = new MyRemoteImpl();
    Naming.rebind("RemoteHello", service);
} catch (Exception ex) {...}
```

### 步骤三：产生Stub和Skeleton

##### 在远程实现类上执行rmic

**rmic**是JDK内的一个工具，用来为一个服务类产生stub和skeleton。

### 步骤四：执行remiregistry

##### 执行rmiregistry

先确定启动目录必须可以访问你的类。最简单的做法是从你的“classes”目录启动。

### 步骤五：启动服务

##### 开启另一个终端，启动服务

从main方法或独立的启动类启动服务。

# 服务器端代码

```java
import java.rmi.*;

public interface MyRemote extends Remote {
    public String sayHello() throws RemoteException;
}


import java.rmi.*;
import java.rmi.server.*;

public class MyRemoteImpl extends UnicastRemoteObject implements MyRemote {
    public String sayHello() {
        return "Server says, 'hey'";
    }

    public MyRemoteImpl() throws RemoteException { }

    public static void main(String[] args) {
        try {
            MyRemote service = new MyRemoteImpl();
            Naming.rebind("RemoteHello", service);
        } catch (Exception ex) {
            ex.printStackTrace();
        }
    }
}
```

# 如何取得stub对象

```java
MyRemote service = (MyRemote)Naming.lookup("rmi://127.0.0.1/RemoteHello");
```

### 工作方式

1. 客户到RMI registry中寻找
2. RMI registry返回Stub对象
3. 客户调用stub的方法，就像stub就是真正的服务对象一样

# 客户端代码

```java
import java.rmi.*;

public class MyRemoteClient {
    public static void main(String[] args) {
        new MyRemoteClient().go();
    }

    public void go() {
        try {
            MyRemote service = (MyRemote)Naming.lookup("rmi://127.0.0.1/RemoteHello");
            String s = service.sayHello();
            System.out.println(s);
        } catch (Exception ex) {
            ex.printStackTrace();
        }
    }
}
```

### 常犯错误

1. 忘记启动远程服务前先启动rmiregistry
2. 忘记变量和返回值类型为可序列化类型
3. 忘记给客户提供stub类

# 改造GumballMachine成远程服务

1. 为GumballMachine创建一个远程接口。提供一组可以远程调用的方法。
2. 确定接口所有返回类型都是可序列化的。
3. 在一个具体类中实现此接口。

### 远程接口

```java
import java.rmi.*;

public interface GumballMachineRemote extends Remote {
    public int getCount() throws RemoteException;
    public String getLocation() throws RemoteException;
    public State getState() throws RemoteException;
}
```

### 返回类型State不是可序列化的

```java
public interface State extends Serializable { // 只要扩展Serializable接口，所有子类中的State就可以在网络上传送了
    public void insertQuarter();
    public void ejectQuarter();
    public void turnCrank();
    public void dispense();
    public String getState();
}
```

如果把State中糖果机的引用也传过去，会导致可以调用糖果机的方法，改变糖果机状态，加上**transient**关键字告诉JVM不要序列化这个字段。

```java
transient GumballMachine gumballMachine;
```

### GumballMachine实现接口

```java
import java.rmi.*;
import java.rmi.server.*;

public class NewGumballMachine extends UnicastRemoteObject implements GumballMachineRemote {
    State soldOutState;
    State noQuarterState;
    State hasQuarterState;
    State soldState;
    State winnerState;

    State state = soldOutState;
    int count = 0;

    String location;

    public NewGumballMachine(String location, int numberGumballs) throws RemoteException {
        soldOutState = new SoldOutState(this);
        noQuarterState = new NoQuarterState(this);
        hasQuarterState = new HasQuarterState(this);
        soldState = new SoldState(this);
        winnerState = new WinnerState(this);
        this.count = numberGumballs;
        if (numberGumballs > 0) {
            state = noQuarterState;
        }

        this.location = location;
    }

    public String getLocation() {
        return location;
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

    State getWinnerState() {
        return winnerState;
    }

    public int getCount() {
        return count;
    }

    public String toString() {
        StringBuffer sb = new StringBuffer();
        sb.append("Machine state: ");
        if (state == soldOutState) {
            sb.append("SOLD OUT.");
        } else if (state == noQuarterState) {
            sb.append("NO QUARTER.");
        } else if (state == hasQuarterState) {
            sb.append("HAS QUARTER.");
        } else if (state == soldState) {
            sb.append("SOLD.");
        } else if (state == winnerState) {
            sb.append("WINNER.");
        }
        sb.append(" Gumball left: " + count + ".");
        return sb.toString();
    }

    public State getState() {
        return state;
    }

    public static void main(String[] args) {
        GumballMachineRemote gumballMachine = null;
        int count;
        if (args.length < 2) {
            System.out.println("GumballMachine <name> <inventory>");
            System.exit(1);
        }

        try {
            count = Integer.parseInt(args[1]);
            gumballMachine = new NewGumballMachine(args[0], count);
            Naming.rebind("//" + args[0] + "/gumballmachine", gumballMachine);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### GumballMonitor客户端

```java
import java.rmi.*;

public class GumballMonitor {
    GumballMachineRemote gumballMachine;

    public GumballMonitor(GumballMachineRemote gumballMachine) {
        this.gumballMachine = gumballMachine;
    }

    public void report() {
        try {
            System.out.println("Gumball Machine: " + gumballMachine.getLocation());
            System.out.println("Current inventory: " + gumballMachine.getCount() + " gumballs");
            System.out.println("Current state: " + gumballMachine.getState().getState());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    public static void main(String[] args) {
        String location = "rmi://localhost/gumballmachine";
        GumballMonitor monitor = null;

        try {
            GumballMachineRemote machine = (GumballMachineRemote)Naming.lookup(location);
            monitor = new GumballMonitor(machine);
            monitor.report();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

# 定义代理模式

**代理模式**为另一个对象提供一个替身或占位符以控制对这个对象的访问。

### 代理控制访问的方式

* 远程代理控制访问远程对象
* 虚拟代理控制访问创建开销大的资源
* 保护代理基于权限控制对资源的访问

### 类图

![proxy_2](http://www.dofactory.com/images/diagrams/net/proxy.gif)

**Subject**为RealSubject和Proxy提供接口。通过实现同一接口，Proxy在RealSubject出现的地方取代它。

**RealSubject**是真正做事的对象，它是被proxy代理和控制访问的对象。

**Proxy**持有RealSubject的引用。客户和RealSubject的交互都必须通过Proxy。

# 虚拟代理(Virtual Proxy)

### 与远程代理对比

##### 远程代理

远程代理可以作为另一个JVM上对象的本地代表。

##### 虚拟代理

虚拟代理作为创建开销大的对象的代表。虚拟代理经常直到我们真正需要一个对象的时候才创建它。

### 显示CD封面

用一个Icon接口从网络上加在图像，虚拟代理代理Icon，管理背景的加载，并在加载未完成时显示“加载中”，一旦加载完成，代理就把显示的职责委托给Icon。

### 设计CD封面虚拟代理

##### Icon接口

* getIconWidth()
* getIconHeight()
* paintIcon()

##### ImageIcon类实现Icon接口

显示图像的类

##### ImageProxy实现Icon接口

代理，首先显示消息，图像加载完成后委托ImageIcon显示图像

##### ImageProxy如何工作

1. 创建ImageIcon，从网络URL上加载图像。
2. 加载过程中，ImageProxy显示“加载中”
3. 加载完毕后，把所有方法调用委托给真正的ImageIcon。
4. 如果用户请求新的图像，我们就创建新的代理。

### 编写ImageProxy

```java
public class ImageProxy implements Icon {
    ImageIcon imageIcon;
    URL imageURL;
    Thread retrievalThread;
    boolean retrieving =false;

    public ImageProxy(URL url) {
        imageURL = url;
    }

    public int getIconWidth() {
        if (imageIcon != null) {
            return imageIcon.getIconWidth();
        } else {
            return 800;
        }
    }

    public int getIconHeight() {
        if (imageIcon != null) {
            return imageIcon.getIconHeight();
        } else {
            return 600;
        }
    }

    public void paintIcon(final Component c, Graphics g, int x, int y) {
        if (imageIcon != null) {
            return imageIcon.paintIcon(c, g, x, y);
        } else {
            g.drawString("Loading CD cover, please waiting...", x + 300, y + 190);
            if (!retrieving) {
                retrieving = true;
                retrievalThread = new Thread(new Runnable() {
                    public void run() {
                        try {
                            imageIcon = new ImageIcon(imageURL, "CD Cover");
                            c.repaint();
                        } catch (Exception e) {
                            e.printStackTrace();
                        }
                    }
                })
                retrievalThread.start();
            }
        }
    }
}


public class Test {
    public static void main(String[] args) {
        ImageComponent imageComponent;

        public static void main(String[] args) {
            Test test = new Test();
        }

        public Test() throws Exception {
            Icon icon = new ImageProxy(initialURL);
            imageComponent = new ImageComponent(icon);
            frame.getContentPane().add(imageComponent);
        }
    }
}
```

# 保护代理

例如：比人不能调用setter方法改变你的个人信息，但是setter是public的，这就是一个保护代理。

### 步骤

##### 1. 创建两个InvocationHandler

实现代理功能，负责代理个人信息（别人不能修改），他人评价（你不能修改）。一个负责拥有者，一个负责非拥有者。

##### 2. 写代码创建动态代理

我们需要写一些代码产生代理类，并实例化它。

##### 3. 利用适当的代理包装任何PersonBean对象

根据情况创建合适的代理。

### 创建InvocationHandle

```java
import java.lang.reflect.*;

public class OwnerInvocationHandler implements InvocationHandler {
    PersonBean person;

    public OwnerInvocationHandler(PersonBean person) {
        this.person = person;
    }

    public Object invoke(Object proxy, Method method, Object[] args) throws IllegalAccessException {
        try {
            if (method.getName().startWith("get")) { // 如果方法是一个getter，我们就调用person内的方法
                return method.invoke(person, args);
            } else if (method.getName().equals("setHotOrNotRating")) { // 如果是setHotOrNotRating，我们抛出异常表示不允许
                throw new IllegalAccessException();
            } else if (method.getName().startWith("set")) { // 如果set，调用person内的方法
                return method.invoke(person, args);
            }
        } catch (InvocationTargetException e) {
            e.printStackTrace();
        }
        return null;
    }
}
```

### 创建Proxy类并实例化Proxy对象

```java
PersonBean getOwnerProxy(PersonBean person) {
    return (PersonBean)Proxy.newProxyInstance(
        person.getClass().getClassLoader(), 
        person.getClass().getInterfaces(),
        newOwnerInvocationHandler(person));
```

### 测试配对服务

```java
import java.rmi.*;
import java.rmi.server.*;

public class Test {
    public static void main(String[] args) {
        Test test = new Test();
        test.drive();
    }

    public Test() {
        initializeDatabase();
    }

    public void drive() {
        PersonBean joe = getPersonFromDatabase("Joe Javabean");
        PersonBean ownerProxy = getOwnerProxy(joe);
        System.out.println("Name is " + ownerProxy.getName());
        ownerProxy.setInterests("bowling, Go");
        System.out.println("Interests set from owner proxy");
        try {
            ownerProxy.setHotOrNotRating(10);
        } catch (Exception e) {
            System.out.println("can't set rating from owner proxy");
        }
        System.out.println("Rating is " + ownerProxy.getHotOrNotRating());

        PersonBean nonOwnerProxy = getNonOwnerProxy(joe);
        System.out.println("Name is " + nonOwnerProxy.getName());
        try {
            nonOwnerProxy.setInterests("bowling, Go");
        } catch (Exception e) {
            System.out.println("can't set interests from non owner proxy");
        }
        nonOwnerProxy.setHotOrNotRating(3);
        System.out.println("Rating set from non owner proxy");
        System.out.println("Rating is " + nonOwnerProxy.getHotOrNotRating());
    }
}
```