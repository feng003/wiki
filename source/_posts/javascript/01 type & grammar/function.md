---
title: "javascript native function (原生函数)"
date: 2016-09-21 16:00
---
> 原生函数

1. String()
2. Number()
3. Boolean()
4. Array()
5. Function()
6. RegExp()
7. Date()
8. Error()
9. Symbol()

    var s = new String('Hello');
    typeof s // object
    s instanceof String //true
    Object.prototype.toString.call(s)  // "[object String]"

> typeof 返回值为 "object"对象都包含一个内部属性 [[Class]]。这个属性无法直接访问，通过  Object.prototype.toString() 来查看

     Object.prototype.toString.call(null) // "[object Null]"
     Object.prototype.toString.call(40)   // "[object Number]"
     Object.prototype.toString.call(true) // "[object Boolean]"


> 所有的函数都可以调用 Function.prototype 中的 call() apply() bind()

    fun.call(thisArg[, arg1[, arg2[, ...]]])
    fun.apply(thisArg, [argsArray])
    apply函数与call的使用场景类似，不同的地方是在调用参数部分，直接给出的是参数数组。把 fun (即this) 绑定到thisArg，这时候thisArg 具备了obj 的属性和方法。或者说 thisArg『继承』了fun的属性和方法。
    fun.bind(thisArg[, arg1[, arg2[, ...]]])
    bind会返回一个改变this指向的新函数

    function f(x,y){
        console.log(x+y);
    }

    f.call(null,6,3)  // 9
    f.apply(null,[4,9])  // 13

    var new_f = f.bind(null,2,5);
    new_f(1);  // 7

**thisArg参数均用null来代替了，在未给出指定thisArg对象的情况下，null与undefined下this指向是全局对象，即js代码执行环境。**

**动态的改变this**

    function add(a, b){console.dir(this); return a+b}
    function sub(a, b){console.dir(this); return a-b;}

    add(1,2);  // Window
    sub(1,2); // Window
    add.call(sub, 1, 2);  //"sub(a, b)"
    sub.apply(add, [1, 2]);  //"add(a, b)"

[call apply bind](https://www.zhihu.com/question/20289071)

[call MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/call)
