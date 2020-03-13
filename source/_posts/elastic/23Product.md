
> setting

##### 关闭 Dynamic Indexes

    PUT _cluster/settings
    {
      "persistent": {
        "action.auto_create_index":false
      }
    }

---

> Elastic Stats API


    Node stats: GET _nodes/stats

    Cluster Stats：GET /_cluster/stats

    Index Stats：GET product/_stats
    

    GET _cluster/pending_tasks

    GET _tasks

    GET _nodes/thread_pool

    GET _nodes/stats/thread_pool

    GET _cat/thread_pool

    GET _nodes/hot_threads
 
---
 
> 集群健康度 绿色状态


    GET _cluster/health

    GET _cluster/health?level=indices

    GET _cluster/health/product

    GET _cluster/health?level=shards

    GET _cluster/allocation/explain

    the shard cannot be allocated to the same node on which a copy of the shard already exists 

    PUT product/_settings
    {
      "number_of_replicas": 0
    }
    
> 提升写性能

##### 关闭无关的功能

    只需要聚合 不需要搜索  index: false
    不需要算分  norms: false (type: text)
    不要对字符串使用默认的 dynamic mapping。字段数量过多，会对性能产生比较大的影响
    Index_options 控制在创建倒排序索引时，哪些内容会被天就到倒排索引中。
    关闭_source 减少IO操作
    
##### 数据写入的过程

    Refresh

    Translog

    Flush

##### 分片设定

---

> 提示读性能


---

> 压力测试

---

> 段合并优化