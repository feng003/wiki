---
title: "javascript value"
date: 2016-09-20 08:50
---

> array 数组   a = []; (length属性 indexOf() concat()方法)

    a = [];
    a[0] = '0';     a[1] = '1';    a[2] = '2';

    a['un'] = 'undefine';

    a.length // 3

    a.forEach(function(e){console.log(e);})

    a.indexOf(1)  // -1
    a.indexOf('1') // 1

    b = a.concat('10');       console.log(b);   // ["0", "1", "2", "10"]

    a.toString()  // "0,1,2"

    d = a.slice(2) // ["2"]

> string 字符串 (类数组)(拥有length属性 以及 indexOf() 和 concat()方法)

    var a = 'look'

    a.charAt(1);  //o

    a.join // undefined
    var c = Array.prototype.join.call(a,'#')   console.log(c) // "l#o#o#k"

    a.map // undefined
    var d = Array.prototype.map.call(a,function(v){
        return v.toUpperCase() + "@";
        }).join("")
    console.log(d)  //  "L@O@O@K@"

    a.reverse()  //  Uncaught TypeError
    a.split("").reverse().join("") // "kool"

> number (数字)

**toFix()**

    var num = 15865974262
    num.toExponential() // 1.5865974262e+10
    var a = 42.59;
    a.toFixed(1)  // 42.6

    42.toFixed(2)  // Uncaught SyntaxError (常量)
    (42).toFixed(2)   // 42.00

**较小的数值**

    0.1+0.2 == 0.3 // false
    function numbersCloseEqual(n1,n2){
        return Math.abs(n1-n2) < Number.EPSILON
    }
    numbersCloseEqual(0.1+0.2,0.3)  // true

**整数检测**

    Number.MAX_VALUE  // 1.7976931348623157e+308
    Number.MAX_SAFE_INTEGER // 9007199254740991   2^53-1

    Number.isInteger(42.000)  // true
    Number.isInteger(42.01)    // false

    Number.isSafeInteger(4.0) // true

**32位有符号整数**

    Math.pow(-2,31) 到 Math.pow(2,31) - 1

> 特殊数值

    null is empty value
    undefined is missing value

**undefined**

    void 1 //undefined

**特殊的数字**

1. NaN

    var a = 2/"foo"
    a  // NaN
    typeof a === 'number'  // true
    isNaN(a) // true

    NaN != NaN // true
    NaN === NaN //false

    if(!Number.isNaN){
        Number.isNaN = function(n){
            return n != n;
        };
    }

2. 无穷数

        var a = 1/0  // Infinity

3. 零值

> 值和引用