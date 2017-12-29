---
title: "javascript this(一)"
date: 2016-05-02 12:40
---

>why use this

    function identify(){
        return this.name.toUpperCase();
    }

    function speak(){
        var greeting = "hello I'm " + identify.call(this);
        console.log(greeting);
    }

    var me = {
        name :"kyle"
    }

    identify.call(me);
    speak.call(me);

this 提供了一种更优雅的方式来隐式“传递”一个对象应用

>mistake

#### 1、this 指向函数自身

    function foo(num){
        console.log("foo:" + num);
        console.log(this);  // window
        this.count++;
    }
    foo.count = 0;
    var i ;
    for(i=0;i<10;i++){
        if(i>5){
            foo(i);
        }
    }
    console.log(foo.count);  // 0

###### 强制 this 指向 foo 函数

    function foo(num){
        console.log("foo:" + num);
        console.log(this);  // foo()
        this.count++;
    }
    foo.count = 0;
    var i ;
    for(i=0;i<10;i++){
        if(i>5){
            foo.call(foo,i); // call确保this 指向函数对象foo 本身
        }
    }
    console.log(foo.count);  // 4

#### 2、this 指向 函数的作用域

    function foo(){
        var a = 3;
        this.bar();
    }
    function bar(){
        console.log(this.a);
    }
    foo();

### this 实际上是在函数被调用时发生的绑定，它指向什么完全取决于函数在哪里被调用。
