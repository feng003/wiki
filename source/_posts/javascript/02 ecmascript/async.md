---
title: "es6之异步操作和async函数"
date: 2017-02-19 12:00
---

> 概念

> Generator函数

1. Generator

2. Generator函数体内外的数据交换和错误处理机制

3. 异步任务的封装

        var fetch = require('node-fetch');

        function* gen(){
            var url = "index.json";
            var result = yield fetch(url);
            console.log(result);
        }

        var g = gen();
        var result = g.next();
        result.value.then(function(data){
            return data.json();
        }).then(function(data){
            g.next(data);
        });

> Thunk函数 求值策略：传值调用(C) 和 传名调用(Haskell)

1. Thunk函数的含义
##### 编译器的“传名调用”实现往往是先将参数放到一个临时函数中，再将这个临时函数传入函数体，这个临时函数就叫做Thunk函数

2. javascript语言的Thunk函数

3. Thunk函数现在用于Generator函数的自动流程管理

> co 模块  用于Generator函数的自动执行

            var fs = require('fs');
            var co = require('co');

            var readFile = function(fileName){
                return new Promise(function(resole,reject){
                    fs.readFile(fileName,function(error,data){
                        if(error) reject(error);
                        resolve(data);
                    });
                });
            };
            co(gen).then(function(){
                console.log('Generator函数传入co函数就会自动执行');
            });


> async 是Generator函数的语法糖

1. async定义

        var fs = require('fs');

        var readFile = function(fileName){
            return new Promise(function(resole,reject){
                fs.readFile(fileName,function(error,data){
                    if(error) reject(error);
                    resolve(data);
                });
            });
        };
        //generator
        var gen = function* (){
            var f1 = yield readFile('/etc/fstab');
            var f2 = yield readFile('/etc/shells');
            console.log(f1.toString());
            console.log(f2.toString());
        };
        //async
        var asyncReadFile = async function(){
            var f1 = await readFile('/etc/fstab');
            var f2 = await readFile('/etc/shells');
            console.log(f1.toString());
            console.log(f2.toString());
        }

##### async函数自带执行器，generator函数的执行靠执行器(co模块)
##### async表示函数有异步操作，await表示紧跟在后面的表达式需要等待结果
##### co模块约定，yield命令后面只能是Thunk函数或Promise对象，而async函数的await命令后面可以是Promise对象和原始类型的值(数值、字符串和布尔值，但这时等同于同步操作)
##### async函数的返回值是Promise对象，可以用then方法指定下一步操作；而Generator函数返回值是Iterator对象

2. async函数的实现

        async function(args){}

3. async函数的用法

        async function getStockPriceByName(name){
            var symbol = await getStockSymbol(name).catch(function(err){
                console.log(err);
            });
            var stockPrice = await getStockPrice(symbol);
            return stockPrice;
        }
        getStockPriceByName('Jan').then(function(reuslt){
            console.log(result);
        });

