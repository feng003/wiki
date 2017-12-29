---
title: "javascript type casting or coercion (类型转换)"
date: 2016-09-25 19:00
---

> 值类型对象

1. implicit coercion(隐式)
2. explicit coercion(显式)

        var a = 42;         typeof a // number
        var b = a + "";   typeof b // string   隐式
        var c = String(a);  typeof c // string  显式

> 抽象值操作

1. ToString

**toString()**

        var a = [1,2,3,4];
        a.toString()

**stringify()**

        JSON.stringify("40")   // ""40""
        JSON.stringify(true)   // "true"
        JSON.stringify(function(){}) // undefined
        JSON.stringify([1,undefined,function(){}])  // "[1,null,null]"

**toJSON()**

2.ToNumber

        Number(undefined)  //  NaN
        Number("aa")         // NaN
        Number(true)       // 1
        Number(false)     // 0
        Number(null)      // 0

3. ToBoolean

> 显式强制类型转换

1. 字符串和数字
2. 显示解析数字字符串
3. 显示转换为布尔值

> 隐式强制类型转换

1. 隐式地简化
2. 字符串和数字
3. 布尔值到数字
4. 隐式强制类型转换为布尔值
5. || 和 &&
6. 符号的强制类型转换

> 宽松相等(==)和严格相等(===)

**"== 允许在相等比较中进行强制类型转换，而 === 不允许"**

> 抽象关系比较
