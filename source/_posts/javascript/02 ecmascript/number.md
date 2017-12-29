---
title: "es6之 Number"
date: 2017-02-23 19:30
---

> 二进制0b(0B) 和 八进制 0o(0O)

                Number('0b111');
                Number('0o10');

> Number.isFinite() Number.isNan()

                Number.isFinite(15);
                Number.isFinite(Infinity);
                Number.isFinite('foo');
                Number.isNaN(10);
                Number.isNaN(NaN);

                //ES5
                (function(global){
                    var global_isNaN = global.isNaN;
                    Object.defineProperty(Number,'isNaN',{
                        value:function isNaN(value){
                            return typeof value === 'number' && global_isNaN(value);
                        },
                        configurable:true,
                        enumerable:false,
                        writable:true
                    });
                })(this);

> Number.parseInt()  Number.parseFloat()

            Number.parseInt === parseInt;
            Number.parseFloat === parseFloat

> Number.isInterger()

            Number.isInteger(3.0)
            Number.isInteger(3.1)
            Number.isInteger('3.1')

> Number.EPSILON

            Number.EPSILON.toFixed(20);

            //浮点数运算误差检查函数
            function withinErrorMargin(left,right){
                return Math.abs(left - right) < Number.EPSILON;
            }
            
            withinErrorMargin(0.1+0.2,0.3)
            withinErrorMargin(0.2+0.2,0.3)

> Number.isSafeInteger()

            Number.MAX_SAFE_INTEGER
            Number.MIN_SAFE_INTEGER

            Number.isSafeInteger('a')
            Number.isSafeInteger(10)
            Number.isSafeInteger(9007199254740993)