---
layout: post
title: Method Invocation Summary
date: 2016-06-07
categories: blog
tags: [study]

---

# 设计模式-命令模式

### 定义

**命令模式**将“请求”封装成对象，以便使用不同的请求、队列或者日志来参数化其他对象。命令模式也支持可撤销的操作。

### 类图

![Command Pattern](http://www.myexception.cn/img/2012/09/14/1713344815.jpg)

### 代码示例

**Client**

```java

public class Test {
    public static void main(String[] args) {
        RemoteControl remoteControl = new RemoteControl();

        Light livingRoomLight = new Light("Living Room");
        Light kitchenLight = new Light("Kitchen");
        CeilingFan ceilingFan = new CeilingFan("Living Room");
        GarageDoor garageDoor = new GarageDoor();
        Stereo stereo = new Stereo("Living Room");

        LightOnCommand livingRoomLightOn = new LightOnCommand(livingRoomLight);
        LightOffCommand livingRoomLightOff = new LightOffCommand(livingRoomLight);
        LightOnCommand kitchenRoomLightOn = new LightOnCommand(kitchenLight);
        LightOffCommand kitchenRoomLightOff = new LightOffCommand(kitchenLight);

        CeilingFanOnCommand ceilingFanOn = new CeilingFanOnCommand(ceilingFan);
        CeilingFanOffCommand ceilingFanOff = new CeilingFanOffCommand(ceilingFan);

        StereoOnWithCDCommand stereoOnWithCD = new StereoOnWithCDCommand(stereo);
        StereoOffCommand stereoOff = new StereoOffCommand(stereo);

        remoteControl.setCommand(0, livingRoomLightOn, livingRoomLightOff);
        remoteControl.setCommand(1, kitchenRoomLightOn, kitchenRoomLightOff);
        remoteControl.setCommand(2, ceilingFanOn, ceilingFanOff);
        remoteControl.setCommand(3, stereoOnWithCD, stereoOff);

        System.out.println(remoteControl);

        remoteControl.onButtonWasPushed(0);
        remoteControl.offButtonWasPushed(0);
        remoteControl.onButtonWasPushed(1);
        remoteControl.offButtonWasPushed(1);
        remoteControl.onButtonWasPushed(2);
        remoteControl.offButtonWasPushed(2);
        remoteControl.onButtonWasPushed(3);
        remoteControl.offButtonWasPushed(3);
    }
}

```

**Invoker**

```java

public class RemoteControl {
    Command[] onCommand;
    Command[] offCommand;
    public static final int COMMAND_NUMBER = 7;

    public RemoteControl() {
        onCommand = new Command[COMMAND_NUMBER];
        offCommand = new Command[COMMAND_NUMBER];

        Command noCommand = new NoCommand();
        for (int i = 0; i < COMMAND_NUMBER; i++) {
            onCommand[i] = noCommand;
            offCommand[i] = noCommand;
        }
    }

    public void setCommand(int slot, Command onCommand, Command offCommand) {
        this.onCommand[slot] = onCommand;
        this.offCommand[slot] = offCommand;
    }

    public void onButtonWasPushed(int slot) {
        onCommand[slot].execute();
    }

    public void offButtonWasPushed(int slot) {
        offCommand[slot].execute();
    }

    public String toString() {
        StringBuffer stringBuff = new StringBuffer();
        stringBuff.append("\n------ Remote Control ------\n");
        for (int i = 0; i < onCommand.length; i++) {
            stringBuff.append("[slot " + i + "] " + onCommand[i].getClass().getName() + "    " + offCommand[i].getClass().getName() + "\n");
        }
        return stringBuff.toString();
    }
}

```

**Receiver**

```java

public class Stereo {
    int volume;
    String position;

    public Stereo(String position) {
        this.position = position;
        volume = 0;
    }

    public void on() {
        System.out.println(position + " stereo is on");
    }

    public void off() {
        System.out.println(position + " stereo is off");
    }

    public void setCD() {
        System.out.println(position + " stereo is set for CD input");
    }

    public void setVolume(int volume) {
        this.volume = volume;
        System.out.println(position + " stereo volume is set to " + volume);
    }
}

public class CeilingFan {
    String position;
    String speed;

    public CeilingFan(String position) {
        this.position = position;
        this.speed = "off";
    }

    public void off() {
        speed = "off";
    }

    public void high() {
        speed = "high";
    }

    public void medium() {
        speed = "medium";
    }

    public void low() {
        speed = "low";
    }

    public void getSpeed() {
        System.out.println(position + " ceiling fan is on " + speed);
    }
}

public class Light {
    String position;

    public Light(String position) {
        this.position = position;
    }

    public void on() {
        System.out.println(position + " light is on");
    }

    public void off() {
        System.out.println(position + " light is off");
    }
}

```

**Command**

```java

public interface Command {
    public void execute();
    public void undo();
}

```

**ConcreteCommand**

```java

public class LightOnCommand implements Command {
    Light light;

    public LightOnCommand(Light light) {
        this.light = light;
    }

    public void execute() {
        light.on();
    }

    public void undo() {
        light.off();
    }
}

public class LightOffCommand implements Command {
    Light light;

    public LightOffCommand(Light light) {
        this.light = light;
    }

    public void execute() {
        light.off();
    }

    public void undo() {
        light.on();
    }
}

public class CeilingFanOnCommand implements Command {
    CeilingFan ceilingFan;

    public CeilingFanOnCommand(CeilingFan ceilingFan) {
        this.ceilingFan = ceilingFan;
    }

    public void execute() {
        ceilingFan.high();
        ceilingFan.getSpeed();
    }

    public void undo() {
        ceiling
    }
}

public class CeilingFanOffCommand implements Command {
    CeilingFan ceilingFan;

    public CeilingFanOffCommand(CeilingFan ceilingFan) {
        this.ceilingFan = ceilingFan;
    }

    public void execute() {
        ceilingFan.off();
        ceilingFan.getSpeed();
    }
}

public class StereoOnWithCDCommand implements Command {
    Stereo stereo;

    public StereoOnWithCDCommand(Stereo stereo) {
        this.stereo = stereo;
    }

    public void execute() {
        stereo.on();
        stereo.setCD();
        stereo.setVolume(11);
    }
}

public class StereoOffCommand implements Command {
    Stereo stereo;

    public StereoOffCommand(Stereo stereo) {
        this.stereo = stereo;
    }

    public void execute() {
        stereo.off();
    }
}

```