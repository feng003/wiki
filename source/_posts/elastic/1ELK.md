---
title: "ELK"
date: 2020-02-17
---

Mysql | Elastic Search
---|---
Database | Index
Table    | Type
Row      | Document
Clolumn  | Field
Schema   | Mapping
Index    | Everything is indexed
SQL      | Query DSL
Select * from table | GET http://
Update table Set    | PUT http://


> ELK = elasticsearch(7.1) + Logstash + kibana + Beats

    搜索、日志分析、指标分析、安全分析

    存储计算
	elasticsearch：后台分布式存储以及全文检索
	
	可视化
	kibana：数据可视化展示
	
	数据抓取
	logstash: 日志加工、搬运工
	Beats: 数据采集器平台
	
	商业  X-Pack
	安全、警告、监考、图查询、机器学习
	
	
```
graph LR
 Beats--> |data-collection| Kafka
redis-->logstash
Kafka--> |buffering| logstash
RabbitMQ-->logstash
logstash--> |data aggregation processing| elasticsearch
kibana-->| analysis visualization |elasticsearch
```


##### 倒排索引、打分机制、全文检索原理、分词原理等底层技术属于“永远不过时”的技术

[es index](https://blog.csdn.net/cyony/article/details/65437708)

[demo](https://elasticsearch.cn/book/elasticsearch_definitive_guide_2.x/_navigating_this_book.html)

[es](https://blog.csdn.net/laoyang360/article/details/79293493)

