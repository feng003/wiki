---
title: "INSTALL"
date: 2020-02-17
---

> logstash是controller层，Elasticsearch是一个model层，kibana是view层。

    首先将数据传给logstash，它将数据进行过滤和格式化（转成JSON格式）
    
    然后传给Elasticsearch进行存储、建搜索的索引
    
    kibana提供前端的页面再进行搜索和图表可视化，它是调用Elasticsearch的接口返回的数据进行可视化
    
    logstash和Elasticsearch是用Java写的，kibana使用node.js框架。
			
	[elast](http://www.echojb.com/network/2017/05/13/384500.html)
	
> java


    设置 $JAVA_HOME

> elasticsearch

##### install


    wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.0.0-linux-x86_64.tar.gz
    wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.4.2-linux-x86_64.tar.gz

    wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.0.0-linux-x86_64.tar.gz.sha512

    shasum -a 512 -c elasticsearch-7.0.0-linux-x86_64.tar.gz.sha512

    tar -xzf elasticsearch-7.0.0-linux-x86_64.tar.gz

    cd elasticsearch-7.0.0/

    run ./bin/elasticsearch

    curl 127.0.0.1:9200
       
##### 目录结构

    bin     脚本文件 包括启动es 安装插件，运行统计数据
    config  elasticsearch.yml 集群配置文件 user role based 相关配置
    JDK     
    data  path.data 数据文件
    lib             Java类库
    logs  path.log  日志文件    
    modules         包含所有ES模块
    plugins         包含已安装插件
    
##### JVM 配置

    修改jvm config/jvm.options
    
    Xmx 和 Xms 设置成一样
    Xmx 不要超过机器内存的 50%
    不要超过 30G
    
#### 运行多个es实例

    bin/elasticsearch -E node.name=node1 -E cluster.name=yushu -E path.data=node1_data -d

    bin/elasticsearch -E node.name=node2 -E cluster.name=yushu -E path.data=node2_data -d
        
    bin/elasticsearch -E node.name=node3 -E cluster.name=yushu -E path.data=node3_data -d

    http://119.29.26.222:9200/_cat/nodes

    http://119.29.26.222:9200/_cat/nodes?v&pretty

    http://119.29.26.222:9200/_cat/health?v&pretty 

    http://119.29.26.222:9200/_cat/indices?v&pretty

    创建索引 curl -XPUT 'localhost:9200/customer?pretty&pretty'

    查询索引 http://119.29.26.222:9200/customer?pretty&pretty 

    删除索引 curl -XDELETE 'localhost:9200/customer?pretty&pretty' 

    从文件 插入数据 curl -H "Content-Type: application/json" -XPOST 'localhost:9200/bank/account/_bulk?pretty&refresh' --data-binary "@accounts.json"

> kibana 数据可视化分析


    wget https://artifacts.elastic.co/downloads/kibana/kibana-7.0.0-linux-x86_64.tar.gz
    wget https://artifacts.elastic.co/downloads/kibana/kibana-7.4.2-linux-x86_64.tar.gz
	
	shasum -a 512 kibana-7.0.0-linux-x86_64.tar.gz
	
	tar -xzf kibana-7.0.0-linux-x86_64.tar.gz

	cd kibana-7.0.0-linux-x86_64/
	
##### dev tools

    ctrl + / 查看api文档
    ctrl + win + l
    ctrl + win + O
    ctrl + win + shift + O
    
##### plugins

    bin/kibana-plugin install plugin_location
    bin/kibana-plugin list
    bin/kibana remove


> logstash 数据处理管道


    实时解析和转化数据


    wget https://artifacts.elastic.co/downloads/logstash/logstash-7.4.2.tar.gz

    wget -qO - <https://artifacts.elastic.co/GPG-KEY-elasticsearch> | sudo apt-key add -  

    sudo apt-get install apt-transport-https 

    echo "deb <https://artifacts.elastic.co/packages/7.x/apt> stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list

    sudo apt-get update && sudo apt-get install logstash

    sudo systemctl start
    
    
	
[ELK Stack+Beats 搭建分布式日志平台](https://www.infvie.com/ops-notes/elkstack-beats.html)


[elasticsearch6.2和logstash启动出现的错误](https://blog.csdn.net/qq_23598037/article/details/79512677)


[logstash](https://www.elastic.co/guide/en/logstash/7.0/running-logstash.html#running-logstash-systemd)


[kibana](https://www.elastic.co/guide/en/kibana/7.0/settings.html)


[kibana](https://www.cnblogs.com/cjsblog/p/9476813.html)