---
title: "es6之Generator"
date: 2017-02-14 12:00
---

> Generator 应用

1. 异步操作的同步化表达

2. 控制流管理

3. 部署Iterator接口

4. 作为数据结构

> Generator简介 generator 可理解成一个状态机，封装了多个内部状态；执行genrator函数会返回一个遍历器对象。


            function* hello(){
                yield "hello";
                yield "js";
                return "node";
            }
            var h = hello();
            h.next();
            h.next();
            h.next();

> next()参数

            function* f()
            {
                for(var i=0;true;i++){
                    var reset = yield i;
                    if(reset){i=-1;}
                }
            }

            var g = f();
            g.next();
            g.next();
            g.next(true);


            function* foo(x){
                var y = 2* (yield (x+1));
                var z = yield (y/3);
                return (x+y+z);
            }

            var a = foo(5);
            a.next();
            a.next();
            a.next();
            var b = foo(5);
            b.next();
            b.next(12);
            b.next(10);

> for...of 循环自动遍历generator函数

            function* numbers(){
                yield 1;
                yield 2;
                return 3;
                yield 4;
            }

            [...numbers()];
            Array.from(numbers());
            let[x,y] = numbers();
            for(let n of numbers()){
                console.log(n);
            }
            //原生js对象没有遍历接口，无法使用for...of
            function* objectEntries(obj){
                let propKeys = Reflect.ownKeys(obj);
                for(let propkey of propKeys){
                    yield [propkey,obj[propkey]];
                }
            }

            let jane = {f:"Jane",l:"Doe"};
            for(let [key,val] of objectEntries(jane)){
                console.log(`${key}:${val}`);
            }

> Generator.prototype.throw() and  Generator.prototype.return()

1. Generator函数返回的遍历器对象都有一个throw方法，可以在函数体外抛出错误，然后在Generator函数体内捕获。 

            var g = function* (){
                while(true){
                    yield;
                    console.log('inner catch',e);
                }
            };
            var i = g();
            i.next();

            try{
                i.throw('a');
                i.throw('b');
            }catch(e){
                console.log('outer catch',e);
            }

2. return 方法可以返回给定的值，并终结Generator函数的遍历

            function* gen(){
                yield 1;
                yield 2;
                return 7;
            }

            var g = gen();
            g.next();
            g.return('gen');
            g.next();

> yield*语句 等同于在Generator函数内部部署一个 for...of循环



> 作为对象属性的Generator函数

        let obj = {
            * myGeneratorMethod(){
                
            }
        }

        let obj = {
            myGeneratorMethod:function* (){
                
            }
        }

> Generator函数的this

        function* F(){
            yield this.x = 1;
            yield this.y = 2;
        }
        //F 返回的是遍历器对象，而不是this对象

        var obj = {};
        var f = F.bind(obj)();
        f.next();
        f.next();

> Generator函数推导 惰性求值

        let bigGenerator = function* (){
            for(let i=0;i<1000;i++){
                yield i;
            }
        };
        let squared = (for(n of bigGenerator()){n*n;});
        console.log(squared.next());






