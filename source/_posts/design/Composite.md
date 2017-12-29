---
title: '5、设计模式之组合模式(Composite)'
date: 2016-10-24 15:50
---

> 问题:

1. 对于树形结构，当容器对象（如文件夹）的某一个方法被调用是，将遍历这个树形结构，寻找也包含这个方法的成员对象并调用执行。（实现文件夹的递归）

2. 客户端希望一致地处理容器对象和叶子对象

> 详解: Composite Pattern (整体-部分模式)

1. 抽象组件类(Component 父类  接口)

2. 叶子节点类(Leaf 子类)

3. 独立集合类(Composite支干 子类)

> php

        <?php
        /**
        * Created by PhpStorm.
        * User: zhang
        * Date: 2016/10/24
        * Time: 17:14
        */

        abstract class MenuComponent
        {
                public $name;
                public abstract function getName();
                public abstract function add(MenuComponent $menu);
                public abstract function remove(MenuComponent $menu);
                public abstract function getChild($i);
                public abstract function show();
        }

        class MenuItem extends MenuComponent{
            public function __construct($name)
            {
                $this->name = $name;
            }

            public function getName()
            {
                // TODO: Implement getName() method.
                return $this->name;
            }

            public function add(MenuComponent $menu)
            {
                // TODO: Implement add() method.
                return false;
            }

            public function remove(MenuComponent $menu)
            {
                // TODO: Implement remove() method.
                return false;
            }

            public function getChild($i)
            {
                // TODO: Implement getChild() method.
                return null;
            }

            public function show()
            {
                // TODO: Implement show() method.
                echo "|".$this->getName()."\n";
            }
            }

        class Menu extends MenuComponent{
                public $menuComponents = array();
                public function __construct($name)
                {
                    $this->name = $name;
                }
                public function getName()
                {
                    // TODO: Implement getName() method.
                    return $this->name;
                }
                public function add(MenuComponent $menu)
                {
                    // TODO: Implement add() method.
                    $this->menuComponents[] = $menu;
                }
                public function remove(MenuComponent $menu)
                {
                    // TODO: Implement remove() method.
                    $key = array_search($menu,$this->menuComponents);
                    if($key !== false){unset($this->menuComponents[$key]);}
                }
                public function getChild($i)
                {
                    // TODO: Implement getChild() method.
                    if(isset($this->menuComponents[$i])) return $this->menuComponents[$i];
                    return null;
                }
                public function show()
                {
                    // TODO: Implement show() method.
                    echo "#".$this->getName()."\n";
                    foreach($this->menuComponents as $v)
                    {
                        $v->show();
                    }
                }
        }

        class testDriver
        {
            public function run()
            {
                $menu1 = new Menu('文学');
                $menuitem1 = new MenuItem('绘画');
                $menuitem2 = new MenuItem('书法');
                $menuitem3 = new MenuItem('小说');
                $menuitem4 = new MenuItem('雕刻');
                $menu1->add($menuitem1);
                $menu1->add($menuitem2);
                $menu1->add($menuitem3);
                $menu1->add($menuitem4);
                $menu1->show();
            }
        }

        $test = new testDriver();
        $test->run();

> javascript


[php代码](http://blog.sina.com.cn/s/blog_50a1e1740101eym1.html)

[参考文档](http://laravelacademy.org/post/2699.html)
