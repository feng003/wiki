---
title: "es6之 Class（语法糖）"
date: 2017-02-23 14:30
---

>  Class syntax

#### ES5通过构造函数定义并生成新对象

                function Point(x,y){
                    this.x = x;
                    this.y = y;
                }
                Point.prototype.toString = function(){
                    return '(' + this.x +','+this.y+')';
                };

#### ES6 class 语法糖

            class Point{
                constructor(x,y){
                    this.x = x;
                    this.y = y;
                }

                toString(){
                    return '(' + this.x +','+this.y+')';
                }
            }

            var point = new Point(3,6);
            point.toString();
            point.hasOwnProperty('x');
            point.hasOwnProperty('toString');
            point.__proto__.hasOwnProperty('toString');

            Point.prototype.constructor === Point  //true

            Point.prototype = {
                 constructor(){},
                 toString(){}
            }

            Object.assign(Point.prototype,{
                toString(){},
                toValue(){}
            })

###### Object.assign方法可以很方便的一次向类添加多个方法

> Class extends

###### ES5的继承是先创造子类的实例对象this，然后再将父类的方法添加到this上(Parent.apply(this))。
###### 而ES6是先创造父类的实例对象this(super方法)，然后再用子类的构造函数修改this

        class ColorPoint extends Point{
            constructor(x,y,color){
                super(x,y);
                this.color = color;
            }
            toString(){
                return this.color + " " + super.toString();
            }
        }

        let cp = new ColorPoint('1','2','red');
        cp instanceof ColorPoint;
        cp instanceof Point

###### Object.getPrototypeOf() 用于从子类上获取父类

        Object.getPrototypeOf(ColorPoint) === Point  //true

> 原生构造函数的继承

#### Boolean()  Number()  String()  Array()  Date()  Function()  RegExp()  Error()  Object()  

            class VersionedArray extends Array{
                constructor(){
                    super();
                }
            }

> Class getter and setter

            class MyClass {
                constructor(){
                    
                }
                
                get prop(){
                    return "getter";
                }
                set prop(value){
                    console.log('setter:' + value);
                }
            }

> Class generator method

> Class static method

#### static 静态方法表示该方法不会被实例继承，而是直接通过类调用

> Class static property

#### Class内部只有静态方法，没有静态属性

> new.target property

> Mixin 多个类的接口“混入”(mix in)另一个类