---
title: "commonjs and module"
date: 2016-09-02 17:10
draft: true
---

> CommonJS定义的模块分为:{模块引用(require)} {模块定义(exports)} {模块标识(module)}

require() 用来引入外部模块；

exports   对象用于导出当前模块的方法或变量，唯一的导出口；

module  对象就代表模块本身。

    var module = {
        exports: {}
    };

    (function(module, exports) {
        exports.multiply = function (n) { return n * 1000 };
    }(module, module.exports))

    var f = module.exports.multiply;
    f(5) // 5000
