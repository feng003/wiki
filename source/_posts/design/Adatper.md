---
title: '4、设计模式之速配器模式(Adapter)'
date: 2016-09-17 15:50
---

>  php 速配器模式实例

#####     速配器 将一个类（对象）的接口（方法或者属性）转化成另一个接口，以满足用户需求，使类（对象）之间的不兼容问题通过速配器得以解决。

##### 应用：数据库操作以及缓存策略

**demo0**

    /**
    * 速配目标 规定的接口将被速配对象实现
    * Interface Database
    */
    interface Database
    {
        public function connect($host,$username,$pwd,$database);

        public function query($sql);

        //    public function insert($sql);
        //    public function find($sql);
        //    public function update($sql);
        //    public function delete($sql);
    }

    /**
    * 速配器
    * Class Mysql
    */
    class Mysql implements Database{
        protected $_connect;

        public function connect($host, $username, $pwd, $database)
        {
            $connect = mysqli_connect($host,$username,$pwd,$database);
            mysqli_select_db($database,$connect);
            $this->_connect = $connect;
            // TODO: Implement connect() method.
        }

        public function query($sql)
        {
            // TODO: Implement query() method.
        }
    }

    /**
    * 速配器
    * Class Mongodb
    */
    class Mongodb implements Database{
        protected $_connect;
        public function connect($host, $username, $pwd, $database)
        {
            $server = "mongodb://".$host.":27017";
            $m = new MongoClient($server); // 连接默认主机和端口为：mongodb://localhost:27017
            $connect = $m->$database;
            $this->_connect = $connect;
            // TODO: Implement connect() method.
        }

        public function query($sql)
        {
            // TODO: Implement query() method.
        }
    }
    $client = new Mysql();
    $client->query($sql);

>  javascript 速配器模式 实例

**demo1**

        var A = A || {};
        window.A = A = jQuery;
        A.g = function(id)
        {
            return $(id).get(0);
        }
        A.on = function(id,type,fn)
        {
            var dom = typeof id === 'string'?$('#'+id):$(id);
            dom.on(type,fn);
        }

        A.on(window,'load',function(){
            A.on('mybutton','click',function(){
                console.log('click');
            })
        })

[php设计模式](http://www.phpddt.com/php/design-adapter.html)
