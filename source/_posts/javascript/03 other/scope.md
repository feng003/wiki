---
title: "javascript scope"
date: 2016-07-14 21:10
---


> javascript 中 无块级作用域

    var name = "name";
    function main(){
        //console.log(name);
        if(1==1){
            var name ="names";
        }
        console.log(name);
    }
    main();

> javascript 采用函数作用域

###### 在JavaScript中每个函数作为一个作用域，在外部无法访问内部作用域中的变量。

    function main(){
        var value ="values";
    }
    main();
    console.log(value);   //     报错：Uncaught ReferenceError: value is not defined

> javascript 的作用域链,根据作用域链从内到外的优先级寻找，如果内层没有就逐步向上找，直到没找到抛出异常。

###### 由于JavaScript中的每个函数作为一个作用域，如果出现函数嵌套函数，则就会出现作用域链。


![image](http://wechat-01.oss-cn-qingdao.aliyuncs.com/YDKJS/scope.png)

> javascript 的作用域链执行前已创建

example 1 作用域链：

    全局作用域 -> Func函数作用域 -> inner函数作用域

    xo = "alex";
    function func(){
        var xo = "eirc";
        function inner(){
            console.log(xo);
        }
        xo = "seven";
        return inner;
    }
    var ret = func();
    ret();  // seven

example 2 两条作用域链：

    全局作用域 -> bar函数作用域
    全局作用域 -> func函数作用域

    xo = "alex";
    function bar(){
        console.log(xo);
    }
    function func(){
        var xo = "seven";
        return bar;
    }
    var ret =func();
    ret();

> 声明提前

    console.log(xo); // Uncaught ReferenceError: xo is not defined

    var xo;
    console.log(xo) //输出：undefined

    function foo(){
        console.log(xo);
        var xo = "seven";
    }
    foo(); //输出：undefined

###### JavaScript的函数在被执行之前，会将其中的变量全部声明，而不赋值。所以，相当于上述实例中，函数在“预编译”时，已经执行了var xo；所以上述代码中输出的是undefined。


[参考地址](http://3060674.blog.51cto.com/3050674/1812390)
