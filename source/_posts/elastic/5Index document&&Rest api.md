
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

> document 文档

    es 是面向文档的，文档是又有可搜索数据的最小单元
    
    文档会被序列化成json格式保存
        json对象由字段组成
        每个字段都有对应的字段类型(字符串、数值、布尔、日期、二进制、范围类型)
    
    每个文档都有一个Unique ID
    
##### 文档的元数据 用于标注文档的相关信息

    _index  文档所属的索引名
    _type   文档所属的类型名
    _id     文档唯一id
    _source 文档的原始json数据
    _version    文档版本信息
    _score  相关性打分
    
> index 索引 是一类文档的集合，是文档的容器

    index 体现了逻辑空间的概念，每个索引都有自己的mapping定义，用于定义包含的文档的字段名和字段类型
    
    shard 体现了物理空间的概念，索引中的数据分散在shard上
    
    Mapping定义文档字段的类型
    Setting定义不同的数据分布
    
##### type 一个索引只能创建一个 type: "_doc"
