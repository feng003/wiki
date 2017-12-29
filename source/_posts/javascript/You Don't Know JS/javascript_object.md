---
title: "javascript object"
date: 2017-09-14 23:40
---
### Object

> 语法

1. 声明

        var obj = {
            key:value
        };

2. 构造

        var myObj = new Object();
        myObj,key = value;


> 类型 string number boolean null undefined object(function array)

1. 内置对象 String Number Boolean Object Function Array Date RegExp Error

        var str = "this is string";
        typeof str;
        var strObj = new String('this is string object');
        typeoff strObj;
        Object.prototype.toString.call(strObj); [object String]

> 内容 属性 属性的名称就像指针一样指向真正的存储位置

1. 可计算属性名（es6）  symbol

2. 属性与方法

3. array

4. 复制对象  copy(); Object.assign({},myObj); Object.create(obj);

5. 属性描述符  value writable enumerable configurable

        var obj = {"a":"a"};
        Object.getOwnPropertyDescriptor(obj,"a") //

        var myObj = {};
        Object.defineProperty(obj,"a",{value:3,writable:true,configurable:true,enumerable:true});

6. 不变性