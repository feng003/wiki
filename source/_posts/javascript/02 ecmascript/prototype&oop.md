---
title: "javascript Ninja 之 prototype and object oriented"
date: 2017-02-24 09:30
---

1. 利用函数实现构造器
2. 探索原型
3. 利用原型实现对象的扩展
4. 避免常见的问题
5. 构建可继承的类

>  instance and prototype

1. javascript 通过构造器执行new操作符来初始化一个新对象

                function Ninja(){};
                Ninja.prototype.swing = function()
                {
                    return true;
                }

                var ninja1 = Ninja();
                var ninja2 = new Ninja();
                typeof ninja1；
                typeof ninja2;

#### 原型可以让我们预定义属性、方法，这些属性和方法会自动应用在新对象实例上

2. 在构造器内的绑定操作优先级永远都高于在原型上的绑定操作优先级

                function Ninja(){
                    this.swung = false;
                    this.swing = function(){
                        return !this.swung;
                    }
                };
                Ninja.prototype.swing = function()
                {
                    return this.swung;
                }
                 var ninja = new Ninja();
                ninja.swing(); // true

3. 协调引用

                    function Ninja(){
                        this.swung = false;
                    };
                    var ninja = new Ninja();
                    Ninja.prototype.swing = function()
                    {
                        return this.swung;
                    }
                    ninja.swing();

#### 引用对象属性时，首先检查对象本身是否拥有该属性，如果没有再查看对象的原型是否有，如果没有返回undefined

4. instanceof 操作符来确定一个实例是否由特定的函数构造器所创建

                function Ninja(){};
                var ninja = new Ninja();
                var ninja2 = new ninja.constructor();
                typeof ninja
                ninja instanceof Ninja
                ninja.constructor  === Ninja

                ninja2 instanceof Ninja
                ninja === ninja2 //false

#### 创建一个原型链最好的方式是，使用一个对象的实例作为另一个对象的原型

5. 继承与原型链 <=> 复制

                function Person(){}
                Person.prototype.dance = function(){};

                function Ninja(){}
                Ninja.prototype = {dance:Person.prototype.dance}; //复制

                var ninja = new Ninja();
                ninja instanceof Ninja
                ninja instanceof Person  //false
                ninja instanceof Object

                 function Person(){}
                Person.prototype.dance = function(){};

                function Ninja(){}
                Ninja.prototype = new Person(); //

                var ninja = new Ninja();
                ninja instanceof Ninja
                ninja instanceof Person  //true
                ninja instanceof Object

#### 所有原生javascript对象构造器(Object、Array、String、Number、RegExp、Function)都有可以被操作和扩展的原型属性

                if(!Array.prototype.forEach){
                    Array.prototype.forEach = function(callback,context){
                        for(var i=0;i<this.length;i++){
                            callback.call(context||null,this[i],index,this);
                        }
                    };
                }
                ['a','s','d'].forEach(function(value,index,array){
                    console.log(value+index+array.length);
                })


> puzzle and trip

1. hasOwnProperty() 方法确定一个属性是在对象实例上定义的，还是从原型里导入的
                            
                Object.prototype.keys = function(){
                    var keys = [];
                    for(var p in this) keys.push(p);
                    return keys;
                };

                var obj = {a:1,b:3,c:5};

                Object.prototype.keys = function(){
                    var keys = [];
                    for(var i in this)
                        if(this.hasOwnProperty(i)) keys.push(i);
                    return keys;
                };

                var obj = {a:1,b:3,c:5};

2.

> 编写类风格的代码

