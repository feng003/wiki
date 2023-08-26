---
title: "0_1SQL简介"
date: 2020-03-11
tag: "简介"
---

> SQL


    I/O操作
    CPU计算 group by order by
    内存使用情况

    //如果在有索引的情况下，表 A 比表 B 大，那么 IN 子查询的效率比 EXISTS 子查询效率高。
    SELECT * FROM A WHERE cc IN (SELECT cc FROM B)

    SELECT * FROM A WHERE EXISTS (SELECT cc FROM B WHERE B.cc=A.cc)
    

    SELECT COUNT(*) as num FROM new_user WHERE TO_DAYS(NOW())-TO_DAYS(regist_time)
    
> 基础


    索引
    排序
    分组
    
> 进阶


    锁
    
> 高级

> 实战


    主从复制
    数据清洗
    数据集成
    数据分析

---

> 数据库优化阶段

- [x] 优化SQL和索引


    慢查询日志定位执行效率低的SQL
    explain 分析SQL的执行计划
- [x] 搭建缓存


    缓存与数据库一致性问题
    缓存击穿、缓存穿透、缓存雪崩
    
[分布式之数据库和缓存双写一致性方案解析](https://mp.weixin.qq.com/s?spm=a2c4e.10696291.0.0.112019a4uQ0rPT&__biz=MzIwMDgzMjc3NA==&mid=2247483670&idx=3&sn=edfb32afe36d7b67c24f1d84df42fa5d&chksm=96f6637fa181ea69f996b48dd3faab8bc74627659153721b018d8d83f7ba947d18d7859eb95d&scene=21#wechat_redirect)

[分布式之redis复习精讲](https://mp.weixin.qq.com/s?spm=a2c4e.10696291.0.0.6b5319a4OZIFtR&__biz=MzIwMDgzMjc3NA==&mid=2247483889&idx=1&sn=dcc00f7767392d55c9e9ee176f7af3dc&chksm=96f66398a181ea8e86a8bca637ce5a80a1792929e2cc4387d4f3fe77fd492bd4ed8ead1cef65&scene=21#wechat_redirect)

- [x] 主从复制 读写分离


    在应用层区分读写请求，或者利用中间件mycat/altas做读写分离
    主从的好处 实现数据库备份、实现数据库负载均衡、提高数据库可用性
    主从的原理
    如何解决主从一致性
    
- [x] 利用分区表 不建议

- [x] 垂直拆分


    把不常用的字段单独放在一张表。

    把常用的字段单独放一张表

    经常组合查询的列放在一张表中（联合索引）。
- [x] 水平拆分


    各模块间耦合性太强，成本太大，慎重。
    
[数据库优化的几个阶段](https://yq.aliyun.com/articles/662394)