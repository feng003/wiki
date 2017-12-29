---
title: "javascript scope(1)"
date: 2016-05-02 12:00
---

###  ”JavaScript中的函数运行在它们被定义的作用域里,而不是它们被执行的作用域里.”　


>##### 一套设计良好的规则来存储变量，并且之后可以方便的找到这些变量。这套规则被称为作用域。


一、传统的编译：

    1、Tokenizing/Lexing 分词/词法分析
    2、Parsing 解析/语法分析
    3、代码生成

二、LHS/RHS  赋值操作的左侧 和 右侧

      RHS查询与简单地查找某个变量的值别无二致，而LHS查询则是试图找到变量的容器本身，从而可以对其赋值。

      RHS理解成 retrieve his source value (取到他的源值)

      赋值操作的目标是谁（LHS） a = 2;
      谁是赋值操作的源头（RHS）console.log(a);

                  function foo(a)
                  {
                        console.log(a) // 2
                  }
                  foo(2);

三、作用域嵌套

    遍历嵌套作用域链的规则：引擎从当前的执行作用域开始查找变量，如果找不到就向上一级继续查找。当抵达最外层的全局作用域时，无论找到还是没有找到，查找过程都会停止。

                  function foo(a)
                  {
                        console.log(a+b);
                  }
                  var b = 2;
                  foo(2);


### 如何管理引擎在当前作用域以及嵌套的子作用域中根据标识符名称进行变量查找？

        1、词法作用域 2、动态作用域(bash)

##### 词法作用域意味着作用域是由书写代码时函数声明的位置来决定的。编译的词法分析阶段基本能够知道全部标识符在哪里以及是如何声明的，从而能够预测在执行过程中如何对它们进行查找。

一、词法阶段    作用域气泡：严格包含

                  function foo(a)                         // 1、全局作用域 标识符foo
                  {
                        var b = a = 2;                   // 2、包含着foo所创建的作用域 三个标识符 a bar b
                        function bar(c)                // 3、包含着bar所创建的作用域 标识符 c
                        {
                              console.log(a,b,c);
                        }
                        bar(b*3);
                  }
                  foo(2);

二、欺骗词法  eval  with 会在运行时修改或创建新的作用域，以此来欺骗其他在书写时定义的词法作用域

                  function foo(str , a)
                  {
                        eval( str );
                        console.log(a ,b);
                  }
                  var b = 2;
                  foo("var b = 3;" , 1);   // 1,3
