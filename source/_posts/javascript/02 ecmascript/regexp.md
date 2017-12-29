---
title: "javascript Ninja 之 regexp"
date: 2017-02-26 10:50
---

1. 正则表达式进修
2. 编译正则表达式
3. 捕捉操作
4. 常用正则表达式

> demo

                /**
                * 99999-9999 判断是否符号美国邮政编码
                * @param candidate
                * @returns {boolean}
                */
                function isThisAZipCode(candidate){
                    if(typeof candidate !== 'string' || candidate.length != 10) return false;
                    for(var n=0;n<candidate.length;n++){
                        var c = candidate[n];
                        switch(n){
                            case 0: case 1: case 2: case 3: case 4: case 6: case 7: case 8: case 9:
                                if(c<'0' || c> '9') return false;
                                break;
                            case 5:
                                if(c !='-') return false;
                                break;
                        }
                    }
                    return true;
                }

                function isThisAZipCode(candidate){
                    return /^\d{5}-\d{4}$/.test(candidate);
                }

> advance

1. regular express 
#### 创建正则表达式：通过正则表达式字面量，或者通过构造RegExp对象的实例

                var pattern = /test/ig;
                var pattern = new RegExp('text','ig');

#### i 让正则表达式不区分大小写 ;g 匹配模式中的所有实例; m 允许匹配多个行

2. term and operate

#### 精确匹配  

                /test/

#### 匹配一类字符 

                [abc]
                [^abc]
                [a-m]

#### 转义 使用 \ 对任意字符进行转义，两个反斜杠 \\ 匹配一个反斜杠。 特殊字符比如 $  .  [  ]  -  ^

#### 匹配开始和结束  ^ and $ 

                 /^test$/

#### 重复出现 默认贪婪，匹配所有的字符组合；非贪婪(操作符后面加一个问号)进行最小限度的匹配。

                /a+/     
                /a+?/
                /t?est/
                /t+est/
                /t*est/
                /a{4}/
                /a{4,10}/
                /a{4,}/

#### 预定义字符类

                    \r
                    \n
                    \d
                    \w
                    \s

#### 分组 用括号进行分组时，同时也创建所谓的捕获

                /(ab)+/ 

#### 或 操作符(OR)

                /(ab)+|(cd)+/

#### 反向引用

                /^(dtn)a\1/
                /<(\w+)>(.+)<\/1>/

> compile regexp express

            function findClassInElements(className,type){
                var elems = document.getElementsByTagName(type || "*");
                var regex = new RegExp("(^|\\s)" + className + "(\\s|$)");
                var result = [];
                for(var i=0,length = elems.length;i<length;i++){
                    if(regex.test(elems[i].className)){
                        result.push(elems[i]);
                    }
                }
                return result;
            }
        
> capture

1. 执行简单的捕获

2. 用全局表达式进行匹配

3. 捕获的引用

4. 没有捕获的分组

> replace 


                "ABCDEFghi".replace(/[A-Z]/g,"X");

                function upper(all,letter){
                    return letter.toUpperCase();
                }
                "border-bottom-width".replace(/-(\w)/g,upper);

                function compress(source){
                    var keys = {};
                    source.replace(/([^=&]+)=([^$]*)/g,function(full,key,value){
                        keys[key] = (keys[key] ? keys[key]+"," : "") + value;
                        return "";
                    });
                    var result = [];
                    for(var key in keys){
                        result.push(key + "=" + keys[key]);
                    }

                    return result.join('&');
                }
                assert(compress("foo=1&foo=3&foo=4&bar=1") == "foo=1,3,4&bar=1","OK");