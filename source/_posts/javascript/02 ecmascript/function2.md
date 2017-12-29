---
title: "javascript Ninja 之function(二)"
date: 2017-02-25 14:30
---

1. 匿名函数
2. 函数调用时的引用形式
3. 函数引用的存储
4. 函数上下文
5. 处理可变长度的参数列表
6. 判断一个对象是否是函数

> 匿名函数

            window.onload = function(){return this;}

            setTimeout(function(){
                console.log(this);
            },1000);

> 递归

1. 普通命名函数中的递归

                //回文检测
                function isPalindrome(text){
                    if(text.length <=1 ) return true;
                    if(text.charAt(0) != text.charAt(text.length - 1)) return false;
                    return isPalindrome(text.substr(1,text.length - 2))
                }

                function chirp(n){
                    return n>1 ? chirp(n-1) +'-chirp' : 'chirp';
                }

2. 方法中的递归

                var ninja = {
                    chirp:function(n){
                        // return n>1?ninja.chirp(n-1) + '-chirp' : 'chirp';
                        return n>1?this.chirp(n-1) + '-chirp' : 'chirp';
                    }
                }

3. 引用的丢失

                var ninja = {
                    chirp:function(n){
                        return n>1?ninja.chirp(n-1) + '-chirp' : 'chirp';
                    }
                }
                var samurai = {chirp:ninja.chirp};
                ninja = {};
                samurai.chirp(5)

4. 内联命名函数 名称(signal)只能在自身函数内部可见

              var ninja = {
                    chirp:function signal(n){
                        return n>1?signal(n-1) + '-chirp' : 'chirp';
                    }
                }
                var samurai = {chirp:ninja.chirp};
                ninja = {};
                samurai.chirp(5)

5. callee属性

> 将函数视为对象

> 可变长度的参数列表

>  函数判断

            function ninja(){}
            typeof ninja;

            function isFunction(fn){
                return Object.prototype.toString.call(fn) === '[object Function]';
            }