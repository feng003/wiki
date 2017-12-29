---
title: '0、设计模式README'
date: 2016-08-29 14:00
---

> 面向对象设计原则

1. 开闭原则(Open-Close Principle OCP)

        open for extension ; closed for modification
        在不修改源代码的基础之上扩展一个系统

2. 依赖倒转原则(Dependency Inversion Principle DIP)

        面向接口（抽象）编程
        禁止具体与具体进行交互

3. 单一职责原则(Single Responsibility Principle SRP)

        类的设计主要工作是"发现职责"并"分离职责"，一个类只负责一个功能领域中的相应职责。

4. 合成复用原则(Composite Recuse Principle CRP)

        组合/聚合复用原则

5. 里氏替换原则(Liskov Substitution Principle LSP)

主要是**针对继承**的设计原则

子类可以扩展父类的功能，但不能改变父类原有的功能

子类**可以实现**父类的抽象方法，但**不能覆盖**父类的非抽象方法

子类中**可以增加**自己特有的方法

当子类的方法重载父类的方法时，方法的前置条件（方法的形参）要比父类方法的**输入参数更宽松**

当子类的方法实现父类的抽象方法时，方法的后置添加（方法的**返回值**）要比父类**更严格**

6. 迪米特法则(Law of Demeter LoD)

        最少知识原则（通过第三者转发调用）

7. 接口隔离原则(Interface Segregation Principle ISP)

        Interface

> 目录

[参考文档](https://www.phpxy.com/article/60.html)

[单例原则](./Singleton.md)

[策略原则](./Strategy.md)

[设计模式](http://www.ibm.com/developerworks/cn/opensource/os-php-designptrns/)
