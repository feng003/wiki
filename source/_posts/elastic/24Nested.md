

##### Denormalization

- [x]     反范式化设计


    数据 flattening 不使用关联关系，而是在文档中保存冗余的数据拷贝
    无需处理join操作，数据读取性能好。es通过压缩_source字段，减少磁盘空间的开销
    不适合在数据频繁修改的场景。一条数据的改动，可能会引发数据的更新
    
- [x]     es中处理关联关系


    关系型数据库，一般会考虑Normalize数据，在es中往往考虑Denormalize数据

    Denormalize的好处：读的速度变快、无需表连接、无需行锁
    
    
###### es通过四种方法处理关联：

- [ ]     对象类型

    
- [ ]     嵌套对象 Nested Object


- [ ]     父子关联关系 Parent Child


- [ ]     应用端关联


    update_by_query

    POST _reindex
    {
        "source":{
            "index":"product_new"
        },
        "dest":{
            "index":"product"
        }
    }
    
##### 对字段进行建模

- [x]     字段类型


    Test 

    用于全文本字段，文本会被analyzer 分词。默认不支持聚合分析以及排序，需要设置fielddata为true

    Keyword

    用于id，枚举以及不需要分词的文本。例如电话号码、email、手机号码、性别。适用于filter(精确匹配) sorting 和 aggregations

    设置多字段类型 

    默认会为文本类型设置成text 并且设置一个keyword的子字段


- [x]     是否要搜索以及分词


    不需要检索、排序和聚合分析 Enable设置成false
    不需要检索 Index设置成false
    需要检索的字段，可以设定存储粒度 index options / norms

- [x]     是否要聚合以及排序


    不需要排序或聚合分析 Doc_values/fielddata设置成 false
    更新频繁的keyword类型的字段 将eager_global_ordinals设置成 true

- [x]     是否要额外的存储

    
    
    
