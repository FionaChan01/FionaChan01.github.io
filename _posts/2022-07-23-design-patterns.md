---
title: 'FiFi‘s Work: Design Patterns'
date: 2022-07-23
permalink: /posts/2022/07/blog-post-design-patterns/
tags:
  - Knowledge
---

HeadFirst设计模式｜学习笔记

> 在微软实习第二周了，觉得自己在整体架构的处理、接口的完善方面都没有掌握技巧。好多代码虽然是实现了功能，但是真的感觉好蠢orz，所以买了这本书，系统性的学习设计模式，感觉蛮有意思的哈哈哈



# Ⅰ 设计模式

## 1. 策略模式 Strategy Pattern

> 定义了算法族，分别封装使他们可以互相替换，让算法的变化独立于算法的客户

## 2. 观察者模式 Observer Pattern

> 定义了对象间的一对多依赖，当一个对象改变状态时，它所有的依赖者收到通知并且自动更新

<img src="https://raw.githubusercontent.com/FionaChan01/FionaChan01.github.io/master/post_image/post_design_patterns/observer.png"/>

- 需要使主题&观察者之间松耦合
	- 主题：用一个共同的接口来更新观察者

- java内置的观察者模式：java.util.Observable
	- ⚠️ 可观察者是一个类而非接口 -> issues：java不支持多重继承，若某类想同时具有Observable类和另一类的行为，会十分困难

- 有多个观察者时，不可以依赖特定的通知次序

## 3. 装饰者模式 Decorator Pattern

> 装饰者模式动态地将责任附加到对象上，若要扩展功能，装饰者提供了比继承更有弹性的替代方案

<img src="https://raw.githubusercontent.com/FionaChan01/FionaChan01.github.io/master/post_image/post_design_patterns/decorator.png"/>

- 装饰者一般对组件的客户时透明的，除非客户程序依赖于组件的具体类型

- 组成

	- 抽象组件/ 具体组件
	- 抽象装饰者/ 具体装饰者

- 例子

	- Starbuzz

	```java
	Beverage beverage = new darkRoast();
	beverage = new Mocha(beverage);
	beverage = new Mocha(beverage);
	beverage = new Whip(beverage);
	```

	- Java I/O: inputStream -> bufferedInputStream -> LineNumberInputStream

- 缺点

	- 导致设计中有大量的小类

## 4. 工厂模式 Factory Pattern

> 工厂模式用来封装对象的创建，并将这样的行为封装在子类中，使得实例化推迟到子类。工厂方法模式通过让子类决定该创建的对象是什么，来达到将对象创建的过程封装的目的。这样，客户程序中关于超类的代码就和子类对象创建代码解耦了
>
> 通过抽象工厂所提供的接口，可以创建产品的家族，利用这个接口书写代码，使其与实际工厂解耦，无须指明具体的类
>
> abstract Product factoryMethod(String type)

- 工厂：处理创建客户的细节，当其他方法需要调用具体pizza时，就变为该工厂的客户

  ```java
  Public class PizzaStore{
  	SimplePizzaFactory factory;
      
      public PizzaStore(SimplePizzaFactory factory){
          this.factory = factory;
      }
      
      public Pizza orderPizza(String type){
          Pizza pizza;
          pizza = factory.createPizza(type);
          pizza.prepare();
          ...
          return pizza;
      }
  }
  ```

- 简单工厂更像是一个编程习惯，把所有的事情在一个地方处理完

  <img src="https://raw.githubusercontent.com/FionaChan01/FionaChan01.github.io/master/post_image/post_design_patterns/simplefactory.jpg"/>

- 工厂方法

  - 使用继承创建对象，扩展一个类，覆盖他的工厂方法，客户只需要知道所使用的抽象类型，子类来决定具体类型，即只负责将客户从具体类型中解耦

- 抽象工厂

  - 通过对象的组合创建一个产品家族的抽象类型，这个类型的子类定义了产品被产生的方法，若要使用工程，需要先实例化它，然后将它传入一些针对抽象类型所写的代码中，可以把一群相关的产品联合起来

  <img src="https://raw.githubusercontent.com/FionaChan01/FionaChan01.github.io/master/post_image/post_design_patterns/compare.jpg"/>

- 抽象工厂模式

  <img src="https://raw.githubusercontent.com/FionaChan01/FionaChan01.github.io/master/post_image/post_design_patterns/abstractfactory.jpg"/>

  ## 5. 单件模式 Singleton Pattern

  [5. The Singleton Pattern: One of a Kind Objects - Head First Design Patterns [Book\] (oreilly.com)](https://www.oreilly.com/library/view/head-first-design/0596007124/ch05.html)

  > 确保只有一个实例被创建，提供一个全局访问点，且不具有全局变量的缺点（必须在程序一开始就创建好对象），单件模式在需要时才创建对象，可以延迟实例化
  >
  > 单件模式确保一个类只有一个实例，并且提供一个全局访问点

- 改善多线程模式下的缺点

  - 在getInstance()方法中加入关键字synchronized，以此来保证每个线程在进入这个方法之前需要等待别的线程离开该方法 -> 每次调用需要同步，降低性能

  - 使用急切创建实例，而不用延迟实例化的做法

    ```java
    public class Singleton {
        private static Singleton uniqueInstance = new Singleton();
        private Singleton();
        publich static Singleton getInstance() {
            return uniqueInstance;
        }
    }
    ```

  - 用双重检查加锁，在getInstance()中减少使用同步

    ```jAVA
    public class Singleton {
        private volatile static Singleton uniqueInstance;
        private Singleton();
        publich static Singleton getInstance() {
            if (uniqueInstance == null){
                // 只有第一次才执行这里的代码
                uniqueInstance = new Singleton();
            }
            return uniqueInstance;
        }
    }
    ```

  ## 6. 命令模式 

  > 将“动作的请求者“从”动作的执行者“对象中解耦，利用命令对象，把请求封装成一个特定对象，动作请求者调用命令对象做相关的工作
  >
  > 将”请求“封装成对象，以便使用不同的请求、队列或者日志来参数化其他对象，命令模式也支持可撤销的操作

- How

  - 命令对象通过在特定接收者上绑定一组动作来封装一组请求，使得对象只暴露出一个execute()方法

  <img src="https://raw.githubusercontent.com/FionaChan01/FionaChan01.github.io/master/post_image/post_design_patterns/command.png"/>

- 命令模式

  ```java
  public interface Command {
      public void execute();
  }
  
  public class LightOnCommand implements Command {
      Light light;
      public LightOnCommand(Light light){
          this.light = light;
      }
      public void execute() {
          light.on();
      }
  }
  
  public class SimpleRemoteControl{ // 遥控器
      Command slot;
      public SimpleRemoteControl() {}
      public void setCommand(Command command){
          slot = command;
      }
      public void buttonWasPressed() {
          slot.execute();
      }
  }
  
  public class RemoteControlTest { // 命令模式的客户
      public static void main(String[] args){
          SimpleRemoteControl remote = new SimpleRemoteControl(); // 调用者是遥控器，传入一个命令对象发出请求
          Light light = new Light(); // 电灯对象，是请求的接收者
          LightOnCommand lightOn = new LightOnCommand(light); // 创建一个命令，将接收者传给他
          
          remote.setCommand(lightOn); // 把命令传给调用者
          remote.buttonWasPressed();
      }
  }
  ```

  

# Ⅱ 设计原则

- 找出应用中可能需要变化之处，将其独立出来，与无须变化的代码不相混（eg：实体类/ 行为类/ 测试类/ ...）

- 针对接口编程，而不是针对实现编程

- 多用组合，少用继承

	> 利用继承设计子类的行为，是在编译时静态决定的，而且所有子类会继承到相同的行为，但如果利用组合扩展对象的行为，就可以在运行时动态的进行扩展

- 为了交互对象间的松耦合设计而努力

- 类应该对扩展开放，对修改关闭

	> 允许类容易扩展，再不修改现有代码的情况下，就可搭配新的行为，这样使设计可弹性应对改变

- 要依赖抽象，不要依赖具体类（依赖倒置原则）

	- 变量不可以持有具体类的引用
	- 不要让类派生自具体类
	- 不要覆盖基类中已经实现的方法

	

