---
title: '9、设计模式之桥接模式(Bridge)'
date: 2016-11-13 14:20
---

> 环境

    通用的日志记录工具 可以通过数据库记录，也可以通过文本文件记录。

> 详解

    为了应对系统多维度的变化

    将抽象部分与实现部分分离，使它们都可以独立的变化。

> 例子

    把日志记录方式和不同平台上的实现分别当作两个独立的部分来对待

    把这两部分之间连接起来

    Bridge 使用了对象组合的方式

> php 桥接模式

        /**
        * 抽象
        * Class ARoad
        */
        abstract class ARoad
        {
            public $car;
            abstract function run();
        }

        /**
        * 具体
        * Class SpeedRoad
        */
        class SpeedRoad extends ARoad
        {
            function run()
            {
                // TODO: Implement run() method.
                $this->car->run();
                echo "speed";
            }
        }

        class StreetRoad extends ARoad
        {
            function run()
            {
                // TODO: Implement run() method.
                $this->car->run();
                echo "street";
            }
        }

        /**
        * 抽象
        * Interface Car
        */
        interface Car
        {
            function run();
        }

        class Jeep implements Car
        {
            function run()
            {
                // TODO: Implement run() method.
                echo "jeep run";
            }
        }

        class Trunk implements Car
        {
            function run()
            {
                // TODO: Implement run() method.
                echo "trunk run";
            }
        }

        $speed = new SpeedRoad();
        $speed->car = new Jeep();
        $speed->run();

        $street = new StreetRoad();
        $street->car = new Trunk();
        $street->run();

> javascript 桥接模式


[代码参考](http://blog.csdn.net/jhq0113/article/details/45441793)
