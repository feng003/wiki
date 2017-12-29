---
title: "1、设计模式之单例模式(Singleton)"
date: 2016-08-27 09:30
---

> 概念（环境、问题、解决方案）

确保一个类仅有一个**唯一的实例**，并且提供一个全局的访问点

解决的问题：类的“独生子女”(类class 和 实例object)

解决方案：

    将构造函数声明成私有类型，屏蔽通过直接实例化的形式来访问

    控制全局只有一个实例的类 -- static

    提供一个可以获取该实例的方法，用于返回类的实例，并保证得到的是同一个对象

    class Singleton
    {
        //静态私有变量保存唯一的实例
        private static $singleton=null;
        // 私有初始化  new Singleton() error
        private function __construct(){

        }

        /**
         * 保证只会实例化一次 单例统一访问入口
         * @return null|Singleton  返回应用中的唯一对象实例
         */
        static public function getInstance(){
            if(is_null(self::$singleton) || isset(self::$singleton)){
                self::$singleton = new self();
            }

            return self::$singleton;
        }
    }

> php 应用

        class Config{

            static private $instance = null;
            private $config = [];
            private __construct(){

            }

            static public function getInstance(){
                if(!self::instance){
                    self::instance = new self();
                }

                return self::instance;
            }

            public function get($key){
                return $this->config[$key];
            }

            public function set($key,$val){
                $this->config[$key] = $val;
            }
        }

> javascript 应用

1. 命名空间

        var Z = {
            g:function(id)
            {
                return document.getElementById(id);
            },
            css:function(id,key,value)
            {
                this.g(id).style[key] = value;
            }
        };

2. 模块化



3. 静态变量(无法修改)

        var Conf = (function(){
            var conf = {
                MAX_NUM : 100,
                MIN_NUM : 1,
                COUNT: 100
            };

            return {
                get : function(name){
                    return conf[name] ? conf[name] : null;
                }
            }
        })();
        var count = Conf.get('COUNT');
        console.log(count);

4. 惰性单例(延迟创建)

        var LazySingle = (function(){
            //单例实例引用
            var _instance = null;
            //单例
            function Single()
            {
                return {
                    publicMethod : function () {console.log('method')},
                    publicProperty : '1.0'
                }
            }
            //获取单例对象接口
            return function (){
                if(!_instance)
                {
                    _instance = Single();
                }
                //返回单例
                return _instance;
            }
        })();
        console.log(LazySingle().publicProperty);
        console.log(LazySingle().publicMethod());


[php参考文章](http://www.cnblogs.com/lh460795/archive/2013/07/30/3225650.html)

[js参考书籍](javascript 设计模式)
