---
title: '2、设计模式之策略模式(Strategy)'
date: 2016-09-02 15:00
---

> 策略模式 一系列算法封装起来，使他们可以互相替换

1. 抽象策略类

2. 具体策略类

3. 环境类

> php 策略模式

        /**
        * Interface Strategy 抽象策略类
        */

        interface Strategy{
            public function computePrice($price);
        }

        /**
        * Class Genernal 具体策略类
        */

        class Genernal implements Strategy{

            public function computePrice($price)
            {
                // TODO: Implement computePrice() method.
                return $price;
            }
        }

        class Middle implements Strategy{

            public function computePrice($price)
            {
                // TODO: Implement computePrice() method.
                return $price * 0.8;
            }
        }

        class High implements Strategy{

            public function computePrice($price)
            {
                // TODO: Implement computePrice() method.
                return $price * 0.5;
            }
        }

        /**
        * Class Price 环境实现类
        */

        class Price{
            // 具体策略对象
            private $strategyInstance;
            // 构造函数
            public function __construct($instance){
                $this->strategyInstance = $instance;
            }

            public function compute($price){
                return $this->strategyInstance->computePrice($price);
            }
        }

        $p = new Price(new High());

        $total = $p->compute(100);

> javascript 策略模式

##### 价格策略对象

    var PriceStrategy = function()
    {
            var stragtegy = {
                return30 : function(price)
                {
                    return price + parseInt(price/100)*30;
                },
                return50 : function(price)
                {
                    return price + parseInt(price/100)*50;
                },
                percent90 : function(price)
                {
                    return price*100*90/10000;
                },
                percent80 : function(price)
                {
                    return price*100*80/10000;
                },
                percent50 : function(price)
                {
                    return price*100*50/10000;
                }
            };
            return function(alg,price)
            {
                return stragtegy[alg] && stragtegy[alg](price);
            }
    }();
    var price = PriceStrategy('return50','200');

##### 表单验证

    var InputStrategy = function()
    {
        var strategy = {
                notNull : function(value)
                {
                        return /\s+/.test(value)?'请输入内容':'';
                },
                number : function(value)
                {
                        return /^[0-9]+(\.[0-9]+)?$/.test(value) ? '' : '请输入数字';
                },
                phone : function()
                {
                        return /^d{3}\-\d{8}$\d{4}\-\d{7}$/.test(value) ? '' : '请输入正确的电话号码格式，如010-12345678';
                }
        }
        return {
            check : function(type,value)
            {
                    value = value.replace(/^s+|\s+$/,'');
                    return strategy[type] ? strategy[type](value) : '没有该类型的检测方法'
            },
            //添加策略
            addStrategy : function(type,fn)
            {
                    strategy[type] = fn;
            }
        }
    }();
    InputStrategy.addStrategy('nickname',function(value){
            return /^[a-zA-z]\w{3,7}$/.test(value) ? '' : '请输入4-8位昵称';
    });

##### 缓冲函数 jq 的animate方法以及 easing.js


#### 对于分支语句的优化，可以通过三种模式：工厂方法模式，状态模式与策略模式。
