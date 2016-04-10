---
layout: post
title: JAVA 设计模式
subtitle:   ""
date:       2013-06-02
author:     "qingtian"
catalog:    true
tags:
    - java
    - 设计模式
---

一、接口型模式介绍

1、Adapter（适配器）模式

     Adapter模式的宗旨就是，保留现有类所提供的服务，向客服提供接口，以满足客户的期望。 
 
2、Facade（外观）模式

     Facade模式的目的在于提供一个接口，使其他子系统更加容易使用
 
3、Composite（组合）模式

     Composite模式的设计意图在于：让用户能够用统一的接口处理单个对象以及对象组合。
    
4、Bridge（桥接）模式

     Bridge模式的意图是将抽象与抽象方法的实现相分离，这样它们就可独自变化。
 
二、责任型模式

1、Singleton（单例）模式

     Singleton模式的宗旨在于确保某个类只有一个实例，并且为其提供一个全局的访问点。
 
2、Observer（观察者）模式

     Observer模式的宗旨是在多个对象之间定义一对多的关系，以便当一个对象状态改变的时候，其他所有依赖这个对象的对象都能够得到通知，并被自动更新。
 
3、Mediator（中介者）模式

     Mediator模式的意图是定义一个对象，该对象将对象集合之间的交互封装起来，使用该模式可以降低对象之间的耦合程度，避免对象之间的显式引用，还可以让对象间的交互独立变化。
 
4、Proxy（代理）模式

     Proxy模式的意图在于为对象提供一个代理或者点位来控制对该对象的访问。
 
5、Chain of Responsibility（责任链）模式

     Chain of Responsibility模式可让每个对象都有一次机会决定自己是否处理请求，以便于避免请求的发送者与接收者之间的耦合。
 
6、Flyweight（享元）模式

     Flyweight模式的主要意图在于通过共享来支持大量的细粒度对象的使用效率。
 
三、构造型模式

1、Builder（构造器）模式

     Builder模式的意图是把构造对象实例的代码逻辑移到要实例化的类的外部。
 
2、Factory Method（工厂方法）模式

     Factory Method模式的主要意图是用于创建对象的接口，同时控制对哪个类进行实例化。
 
3、Abstract Factory（抽象工厂）模式

     Abstract Factory模式的意图在于创建一系列相互关联或相互依赖的对象。
 
4、Prototype（原型）模式

     Prototype模式不通过实例化类来创建一个新的未初始化的实例，而是通过复制一个现有对象来生成的对象。
 
5、Memento（备忘录）模式

     Memento模式的意图在于为对象提供状态存储和状态恢复功能。
 
四、操作型模式

1、Template Method（模板方法）模式

     Template Method模式的目的就是在一个方法中实现一个算法，并将算法中某些步骤的实义推迟，从而使得其他类可以重新定义这些步骤。     
 
2、State（状态）模式

     State模式的意图在于将与状态有关的处理逻辑分散到代表对象状态的各个类中。
 
3、Strategy（策略）模式

     Strategy模式的意图在于把可选的策略或方案封装到不同的类中，并在这些类中实现一个共同的操作。
 
4、Command（命令）模式

     Command模式的意图是把对象封装在对象中。
 
5、Interpreter（解释器）模式

     Interpreter模式的意图是可以按照自己定义的组合规则集合来组合可执行对象。
     
 
五、扩展型模式

1、Decorator（装饰器）模式

     Decorator模式的意图是在运行时组合操作的新变化。
 
2、Iterator（迭代器）模式

     Iterator模式的意图在于为开发人员提供一种顺序访问集合元素的方法。
 
3、Visitor（访问者）模式

     Visitor模式的意图在于让代码用户能够在不修改现在类层次结构的前提下，定义该类层次结构的操作。

引用来自：[http://xuzhfa123.iteye.com/blog/1881133](http://xuzhfa123.iteye.com/blog/1881133)