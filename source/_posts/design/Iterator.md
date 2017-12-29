---
title: '设计模式之迭代器模式(Iterator)'
date: 2016-11-14 20:20
---

> 环境

    访问一个聚合对象的内容 而无需暴露它的内部表示

    支持对聚合对想多种遍历

    为遍历不同的聚合结构提供一个统一的接口(支持多态迭代)

> 详解

    提供一种方法访问一个容器对象中各个元素，而又不需暴露该对象的内部细节

四种角色：

    1. 抽象集合  接口，规定了具体集合需要实现的操作

    2. 具体集合

    3. 抽象迭代器  接口，规定了遍历具体集合的方法

    4. 具体迭代器

小结：

    迭代器模式是与集合共生共死的

    语言在实现容器的时候都给提供了迭代器

> php 迭代器模式

            abstract class Iterators
            {
                    public abstract function First();

                    public abstract function Next();

                    public abstract function IsDone();

                    public abstract function CurrentItem();
            }

            class ConcreteIterator extends Iterators
            {
                    private $aggre;
                    private $_current = 0;

                    public function __construct(array $_aggre)
                    {
                        $this->aggre = $_aggre;
                    }

                    public function First()
                    {
                        return $this->aggre[0];
                    }

                    public function Next()
                    {
                        $this->_current++;
                        if($this->_current < count($this->aggre))
                        {
                            return $this->aggre[$this->_current];
                        }
                        return false;
                    }

                    public function IsDone()
                    {
                        return $this->_current >= count($this->aggre)?true:false;
                    }

                    public function CurrentItem()
                    {
                        return $this->aggre[$this->_current];
                    }
            }

            $iterator = new ConcreteIterator(array('A','B','C'));
            $item = $iterator->First();
            echo $item."<br />";
            while(!$iterator->IsDone())
            {
                echo $iterator->CurrentItem().": is what? <br />";
                $iterator->Next();
            }

> javascript 迭代器模式

        var Iterator = function(items,container)
        {
                var container = container && document.getElementById(container)||document,
                item = container.getElementsByTagName(items),
                length = item.length,
                index = 0;
                var splice = [].splice;
                return {
                first : function(){
                    index = 0;
                    return items[index];
                },
                second : function(){
                    index = length-1;
                    return items[index];
                },
                pre : function(){
                    if(--index >0){
                        return items[index];
                    }else{
                        index = 0;
                        return null;
                    }
                },
                next : function(){
                    if(++index<length)
                    {
                        return items[index];
                    }else{
                        index = length-1;
                        return null;
                    }
                },
                get :function(num){
                    index = num >= 0 ? num%length : num%length +length;

                    return items[index];
                },
                dealEach : function(fn){
                    var args = splice.call(arguments,1);
                    for(var i=0;i<length;i++)
                    {
                        fn.apply(items[i],args);
                    }
                },
                dealItem : function(){},
                exclusive : function(){}
                }
        }
