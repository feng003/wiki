---
title: "javascript scope(2)"
date: 2016-05-02 12:10
---

>1、函数作用域

            function foo(a)
            {
                    var b = 2;
                    function bar(){}
                    var c = 3;
            }

>2、隐藏内部实现

            function doSth(a)
            {
                b = a + doSthElse( a * 2);
            }
            function doSthElse(a)
            {
                return a-1;
            }
            var b;
            doSth(2);

隐藏之后

        function doSth(a)
        {
                function doSthElse(a)
                {
                    return a-1;
                }

                var b;

                b = a + doSthElse(a * 2);
                console.log(b * 3);
        }
        doSth(2);

         b 和 doSthElse() 无法从外部访问。

 >3、函数作用域 (匿名函数自执行)

            var a = 2;
            (function foo(){
                var  a = 3 ;
                console.log( a );    // 3
                })();
            console.log(a);      //2

>4、块作用域  with、try/catch、let



### 提升 变量和函数声明从他们在代码中出现的位置被“移动”到最上面。

>1、编译器

 foo函数的声明被提升

            foo();
            function foo()
            {
                console.log(a);
                var a = 2;
            }

函数表达式却不会被提升

            foo();

            var foo = function(){}

>2、函数优先:函数声明 会被提现到 普通变量之前

            foo();
            var foo;
            function foo()
            {
                console.log(1);
            }
            foo = function()
            {
                console.log(2);
            }
