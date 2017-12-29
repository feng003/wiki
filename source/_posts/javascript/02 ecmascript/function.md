---
title: "javascript Ninja 之function(一)"
date: 2017-02-24 14:00
---
1. 理解函数
2.  函数为什么是第一型对象
3. 浏览器如何调用函数
4. 函数声明
5. 参数赋值
6. 函数上下文

> function declare

1. 
            function isNimble(){
                return true;
            }

            var canFly = function(){
                return true;
            }

            function outer(){
                typeof inner;
                function inner(){
                    return true;
                }    
                window.inner;
            }

            typeof window.isNimble;
            isNimble.name;

2. scope and function

            if(window){
                var x = 123;
            }
            alert(x);

#### 变量声明的作用域开始于声明的地方，结束于所在函数的结尾，与代码嵌套无关
#### 命名函数的作用域是指声明该函数的整个函数范围，与代码嵌套无关(机制提升)
#### 对于作用域声明，全局上下文就像一个包含页面所有代码的超大型函数

> function implicit parameter: arguments and this 

#### arguments 参数是传递给函数的所有参数的一个集合，非数组。
#### this参数引用了与该函数调用进行隐式关联的一个对象，称之为函数上下文

> function invoking

#### 函数调用方式之间的主要差异是：作为this参数传递给执行函数的上下文对象之间的区别。作为方法进行调用，该上下文是方法的拥有者；作为全局函数进行调用，其上下文是window；作为构造器进行调用，其上下文对象则是新创建的对象实例。

1. 作为一个函数进行调用

                function f(){return this;}
                f() === window;

2. 作为一个方法进行调用(对象)

                function creep(){return this;}
                var sneak = creep;
                var ninja1 = {skulk:creep};
                var ninja2 = {skulk:creep};

3. 作为构造器进行调用

                function creep(){return this;}
                new creep() 

4. 通过apply()或call()方法进行调用，可以自由执行函数上下文。

#### 使用apply() call() 方法可以显式指定任何一个对象作为其函数上下文

                function juggle(){
                    var result = 0;
                    for(var n=0;n<arguments.length;n++){
                        result += arguments[n];
                    }
                    return this.result = result;
                }
                var ninja1 = {};
                var ninja2 = {};

                juggle.apply(ninja1,[1,2,3,4]);
                juggle.call(ninja2,1,2,3);

#### 在回调中强制指定函数上下文

                function forEach(list,callback){
                        for(var n=0; n<list.length; n++){
                            callback.call(list[n],n);
                        }
                    }
                    var wanpons = ['s','t','n'];
                    forEach(wanpons,function(index){
                        console.log(index);
                        assert(this===wanpons[index],'value'+wanpons[index]);
                    })


