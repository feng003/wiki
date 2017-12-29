---
title: " vue TODO"
date: 2017-04-26 18:30
draft: true
tags: vue
---
## TODO

1. vuex 现在简单的理解就是一个获取数据  存储数据(状态是什么东西？)  store存储的 state 可以展示与页面 与页面交互


            state  => mutations => actions   store.dispatch()  顺序以及他们之间的关系？ 

2. vue 路由简单理解了  都是单页面式的 不需要url进行传参       

    
            require.ensure  实现了一个懒加载   还没搞清楚原理 是什么？

3. .vue 后缀的 相当于 html 页面。通过 template script style来标记，在js中，通过 export default {} 来实例vue  没搞清楚 是什么原理？

4. export default 包括   


        data 初始化数据  
        methods 所有的方法都可以写在这里面    
        mounted 页面预加载执行的方法     
        computed 可以接受vuex 中的state 别的作用就没用到了 

5. API 

> 选项/数据  选项/DOM  选项/生命周期钩子  选项/资源  选项/杂项

> 实例属性  实例方法/数据 实例方法/事件 实例方法/生命周期

> 指令  特殊属性  内置的组件

6. vue 数据绑定

        v-model
        v-on/@
        v-bind/:

        v-if
        v-for
        v-show

7. vue component  组件

        props
        event
