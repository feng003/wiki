---
title: "es6之Iterator"
date: 2017-02-18 16:00
---

> Iterator 遍历器对象本质上就是一个指针对象，是一种接口，为不同的数据结构提供统一的访问机制。

1. 为各种数据结构提供一个统一的、简便的访问接口
2. 使得数据结构的成员能够按某种次序排列
3. for...of循环

        function makeIterator(array){
            var nextIndex = 0;
            return {
                next:function(){
                    return nextIndex < array.length ?
                    {value:array[nextIndex++],done:false} :
                    {value:undefined,done:true};
                }
            }
        }
        var it = makeIterator(['a','b']);
        it.next();

##### 凡是部署了Symbol.iterator属性的数据结构，就称为部署了遍历器接口。调用这个接口，就会返回一个遍历器对象。(数组具备Iterator接口，而对象不具备)

> 数据结构的默认Iterator接口

##### es6中，三类数据结构原生具备Iterator接口：数组、某些类似数组的对象以及Set和Map结构

        let arr  = ['a','v','b'];
        let iter = arr[Symbol.iterator]();
        iter.next();

> 调用Iterator接口的场合

1. 解构赋值

        let set = new Set().add('a').add('b').add('c');
        let [x,y,z,] = set

2. 扩展运算符

        var str = "javascript";
        [...str];

> 字符串的Iterator接口

        var str = "javascript";
        typeof str[Symbol.iterator];
        var iterator = str[Symbol.iterator]();
        iterator.next();
        iterator.next();

> Iterator接口与Generator函数


> 遍历器对象的 return()、throw()

> for...of循环



