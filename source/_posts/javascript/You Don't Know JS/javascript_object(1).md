---
title: "javascript object(1)"
date: 2016-05-02 12:50
---

> syntax  声明(文字)形式 和 构造形式

    var myObj = {
        key : value
    };

    var myObj = new Object();
    myObj.key = value;

> types

##### string number boolean null undefined object (function array)

##### 内置对象 String Number Boolean Object Function Array Date RegExp Error

    var str = " I am string";
    typeof str // string
    str instanceof String // false

    var strObj = new String("I am object");
    typeof strObj   // object
    strObj instanceof String // true

    Object.prototype.toString.call(strObj);

> loop  for..in 遍历对象的可枚举属性列表（包括[[prototype]]链）

    for..of

> Content (property)

    var myObj = {
        a:'22'
    }

    myObj.a; // 属性访问
    myObj['a'] //键访问

#### 可计算属性名(ES6 Symbol)

    var prefix = "foo";
    var myObj = {
        [prefix + "bar"] : "hello",
        [prefix + "baz"] : "world"
    };

    myObj["foobar"];

#### 属性与方法

    function foo(){
        console.log('foo' + this);
    }
    var someFoo = foo;
    var myObj = {
        someFoo : foo
    }
    foo;
    someFoo;
    myObj.someFoo;
    someFoo()    //  foo[object Window]
    myObj.someFoo()  // foo[object Object]

    var newObj = {
        foo:function(){
            console.log('new foo' + this);
        }
    }
    var newFoo  = newObj.foo;
    newFoo;
    newObj.foo;

#### array

    var myArr = ["foo",10,"bar",true];
    myArr.length  //4
    myArr[0]      //foo

###### 对象来存储 键/值对，而数组来存储 数值下标/值对

#### 复制对象

    function anotherFunc(){}
    var anotherArr = [];
    var anotherObj = {a:true};
    var myObj = {
        a:2,
        b:anotherObj,  //引用 不是复本
        c:anotherArr,  // 另一个引用
        d:anotherFunc
    }
    anotherArr.push(anotherObj,myObj);

###### 对于浅复制 ES6定义了  Object.assign() 来实现，第一个参数是目标对象，之后可以跟一个或多个源对象

    var newObj = Object.assign({},myObj)；

    newObj;
    newObj.b === anotherObj;   // true

#### 属性描述符

    var myObj = {a:2};
    Object.getOwnPropertyDescriptor(myObj,"a")   // Object {value: 2, writable: true, enumerable: true, configurable: true}

    Object.defineProperty(myObj,"a",{value:3,writable:false,enumerable:true,configurable:true})

1. writable 是否可以修改属性的值

2. Configurable  属性是可配置的；除了无法修改，Configurable:false 还会禁止删除这个属性

3. Enumerable

描述符控制的是属性是否会出现在对象的属性枚举中，比如说 for .. in 循环。如果把enumerabel 设置成false，这个属性就不会出现在枚举中。

#### 不变性

1. 对象常量 结合writable:false 和 configurable:false就可以创建一个真正的常量属性(不可修改、重定义或删除)

    var myObj = {}
    Object.defineProperty(myObj,"FACORITE_NUMBER",{value:40,writable:false,configurable:false})

    myObj //Object {FACORITE_NUMBER: 40}

2. 禁止扩展  Object.preventExtensions()

    var newObj = {a:3}
    Object.preventExtensions(newObj)
    newObj.b = 4 ;
    newObj.b   // undefined

3. 密封 Object.seal()

4. 冻结 Object.freeze()


#### [[Get]]

对象默认的内置[[Get]]操作首先在对象中查找是否有名称相同的属性，如果没有找到，就会变量可能存在的[[Prototype]]链

    var myObj = {a:undefined}
    myObj.a   // undefined
    myObj.b  // undefined

#### [[Put]]

    如果已经存在这个属性，[[Put]]算法：
    1. 属性是否是访问描述符？是并且存在setter 就调用setter
    2. 属性的数据描述符中writable 是否是false？是，在非严格模式下静默失败；严格模式下抛出TypeError异常
    3. 如果都不是，将该值设置为属性的值

#### Getter and Setter

#### 存在性
