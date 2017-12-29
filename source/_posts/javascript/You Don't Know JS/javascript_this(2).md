---
title: "javascript this(二)"
date: 2016-08-01 07:40
---
### this全面解析

> 调用位置：函数在代码中被调用的位置（而不是声明的位置）

    function baz(){
        console.log('baz');
        bar();
    }

    function bar(){
        console.log('bar');
        foo();
    }

    function foo(){
        debugger; //断点调试
        console.log('foo');
    }

    baz();

> 绑定规则

1. 默认绑定：独立函数调用

    function foo(){
        console.log(this.a);
    }
    var a =2 ;
    foo();

2. 隐式绑定：调用位置是否有上下文对象，或者说是否被某个对象拥有或者包含

    function foo(){
        console.log(thia.a);
    }
    var obj = {
        a:2,
        foo:foo
    }
    obj.foo();

3. 显示绑定

4. new 绑定

> 优先级

    1. 存在 new 绑定的话，this绑定的是新创建的对象
        var bar = new foo();

    2. call apply(显示绑定)或者硬绑定调用，this绑定的是指定的对象
        var bar = foo.call(obj2);

    3. 存在某个上下文对象中调用(隐式绑定)，this绑定的是那个上下文对象
        var bar = obj1.foo();

    4. 默认绑定
        var bar = foo();

> 绑定例外


> this 词法

    箭头函数会继承外层函数调用的this 绑定（self = this）

    function foo(){
        return (a) => {
            console.log(this.a);
        }
    }
    var obj1 = {
        a:1
    }
    var obj2 = {
        a:2
    }
    var bar = foo.call(obj1);
    bar.call(obj2);
