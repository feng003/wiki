---
title: "es6之Promise"
date: 2017-02-16 08:00
---

> Promise的应用

1. 加载图片

        const preloadImage = function(path){
            return new Promise(function(resolve,reject){
                var image = new Image();
                image.onload = resolve;
                image.onerror = reject;
                image.src = path;
            })
        }

2. Generator 与 Promise 的结合

> Promise 定义和用法

#### 所谓promise，就是一个对象，用来传递异步操作的消息

1. 对象的状态不受外界影响，三种状态：pending、resolved、rejected
2. 一旦状态改变就不会再变，任何时候都可以得到这个结果。

#### Pormise对象是一个构造函数

        var promise = new Promise(function(resolve,reject){
    
            if(true){
                resolve(value); //将pending变成resolved
            }else{
                reject(error); //将pending变成rejected
            }
        })

        promise.then(function(value){
            //success;
        },function(value){
            //failure;
        })

        function timeout(ms){
            return new Promise((resolve,reject)=>{
                setTimeout(resolve,ms,'done');
            });
        }
        timeout(1000).then((value)=>{
            console.log(value);
        });

        function loadImageAsync(url){
            return new Promise(function(resolve,rejct){
                var image = new Image();
                image.onload = function(){
                    resolve(image);
                };
                image.onerror = function(){
                    reject(new Error('Could not load image at' + url));
                };
                image.src = url;
            });
        }

        var getJSON = function(url){
            var promise = new Promise(function(resolve,reject){
                var client = new XMLHttpRequest();
                client.open('GET',url);
                client.onreadystatechange = handler;
                client.responseType = "json";
                client.setRequestHeader("Accept","application/json");
                client.send();
                
                function handler(){
                    if(this.readyState !==4){
                        return;
                    }
                    if(this.status === 200){
                        resolve(this.response);
                    }else{
                        reject(new Error(this.statusText));
                    }
                }
            });
            
            return promise;
        };

> Promise.prototype.then() and Promise.prototype.catch()

1. then 为Promise实例添加状态改变时的回调函数；then方法返回一个新的Promise实例

        getJSON('/post/1.json').then(function(post){
            return getJSON('post.commentURL');
        }).then(function funcA(comments){
            console.log('Resolved:',comments);
        },function funcB(err){
            console.log('Rejected:',err);
        });

2. catch方法是 .then(null,rejection)的别名

        getJSON('/post/1.json').then(function(post){
            return getJSON('post.commentURL');
        }).then(function funcA(comments){
            console.log('Resolved:',comments);
        }).catch(function(error){
            console.log('Rejected',error);
        });

> Promise.all() 、Promise.race() 、Promise.resolve() 和 Promise.reject()

1. Promise.all() 将多个Promise实例包装成一个新的Promise实例

2. Promise.race() 