---
title: '8、设计模式之状态模式(State)'
date: 2016-11-13 10:20
---

> 解析

    状态模式 是一种行为模式，是面向接口编程原则的体现。“接口----实现类”

> 环境

    状态模式解决的问题是“ 如何通过改变一个对象的状态，修改对象的行为 ”

    酒店房间管理系统     房间状态：空闲、预定、入住

背景：

    1. 某对象发生变化时，其所能做的操作也随之变化。

    2. 应用程序的可维护性和重用性差

    3. 代码的逻辑较复杂

> 详解

角色：

    1. 环境类 (Context) 客户使用的对象类

    2. 抽象状态类 (State) 一个抽象以封装与Context的一个特定状态相关的行为

    3. 具体状态类  (ConcreteState) 每一子类实现一个与Context的一个状态相关的行为

步骤：

    1.

扩展场景

    1. 用户状态管理

    2. 订单状态管理

    3. 开关的状态管理

> php

        interface State
        {
            public function handle($state);
            public function display();
        }

        class Context
        {
            private $_state = null;

            public function __construct($state)
            {
                $this->setState($state);
            }

            public function setState($state)
            {
                $this->_state = $state;
            }

            public function request()
            {
                $this->_state->display();
                $this->_state->handle($this);
            }
        }

        class StateA implements State
        {
            public function handle($context)
            {
                // TODO: Implement handle() method.
                $context->setState(new StateB());
            }

            public function display()
            {
                // TODO: Implement display() method.
                echo "StateA <br />";
            }
        }

        class StateB implements State
        {
            public function handle($context)
            {
                // TODO: Implement handle() method.
                $context->setState(new StateC());
            }

            public function display()
            {
                // TODO: Implement display() method.
                echo "StateB <br />";
            }
        }

        class StateC implements State
        {
            public function handle($context)
            {
                // TODO: Implement handle() method.
                $context->setState(new StateA());
            }

            public function display()
            {
                // TODO: Implement display() method.
                echo "StateC <br />";
            }
        }

        $objContext = new Context(new StateB());
        $objContext->request();
        $objContext->request();
        $objContext->request();
        $objContext->request();
        $objContext->request();

> javascript


[代码参考](http://www.nowamagic.net/librarys/veda/detail/1605)

[参考资料](http://blog.csdn.net/jhq0113/article/details/46439127)
