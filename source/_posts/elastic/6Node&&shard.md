> 分布式的可用性与扩展性

- [x]     高可用性


    服务可用性 提高系统的可用性，部分节点停止服务，整个集群的服务不受影响
    数据可用性 部分节点丢失，不会丢失数据

- [x]     可扩展性


    存储的水平扩容 支持 PB级数据
    请求量提升、数据的不断增长(将数据分布到所有节点上)


- [x]     es分布式架构


    不同的集群通过不同的名字来区分
    Elasticsearch 通过配置文件或者 命令行 -E cluster.name=yushu 设定集群
    一个集群可以有一个或多个节点

    su elk -s /bin/bash -c './elasticsearch -E node.name=yushu1 -E cluster.name=cluster_yushu -E path.data=node1_yushu -E http.port=9201'

    su elk -s /bin/bash -c './elasticsearch -E node.name=yushu2 -E cluster.name=cluster_yushu -E path.data=node1_yushu -E http.port=9202'

    ./elasticsearch -E node.name=yushu1 -E cluster.name=cluster_yushu -E path.data=node1_yushu -E http.port=9201

    ./elasticsearch -E node.name=yushu2 -E cluster.name=cluster_yushu -E path.data=node2_yushu -E http.port=9202


> 节点 是一个es实例  Cerebro 


    本质上就是一个java进程
    一个机器上可以运行多个es进程
    

    每个节点都有名字，通过配置文件配置 或者  命令行 -E node.name=yushu1 设定
    每个节点在启动后，会分配一个UID，保存在data目录下


##### Coordinating Node 处理请求的节点

    路由请求到正确的节点，例如创建索引的请求，需要路由到Master 节点
    所有节点默认都是 Coordinating Node
    通过将其他类型设置成false 使其成为 Dedicated Coordinating Node
    

##### Data node 保存数据的节点

    节点启动后，默认就是数据节点 可以设置 node.data:false 禁止
    保存分片数据  由Master Node 决定如何把分片分发到数据节点上
    通过增加数据节点 可以 解决数据水平扩展 和 解决数据单点 问题


##### Master node

    创建 删除索引等请求，决定分片被分配到哪个节点，负责索引的创建与删除
    维护并更新 Cluster State

    在部署上需要考虑解决单点的问题
    为一个集群设置多个 Master节点，每个节点只承担 Master的单一角色


##### Master-eligible nodes Master Node


    一个集群，支持配置多个Master Eligible节点，这些节点可以在必要时(如master节点出现故障 网络故障时)参与选主流程 成为master节点

    每个节点启动后，默认就是一个 master eligible节点 ，可以设置node.master:false禁止

    当第一个节点启动时候，会将自己选举成master节点
    每个节点都保存了集群的状态，只有master节点才能修改集群的状态信息
    
- [ ] 集群状态，维护了一个急群众，必要的信息


    所有的节点信息
    所有的索引和其相关的Mapping 与 Setting信息
    分片的路由信息

- [ ] 任意节点都能修改信息会导致数据的不一致性


#####  一个节点在默认情况下会同时扮演：mater eligible,data node and ingest node

节点类型 | 配置参数 | 默认值
---|---|---
master-eligible       | node.master | true
data                  | node.data   | true
ingest                | node.ingest | true
coordinating only     | 无          |
Machine Learning      | node.ml     | true

    单一职责的节点 一个节点只承担一个角色
    Master节点 负责集群状态(cluster state)的管理 (低配置)
        node.master:true
        node.ingest:false
        node.data:false
    Data节点 负责数据存储及处理客户端请求 (高配置)
        node.master:false
        node.ingest:false
        node.data:true
    Ingest节点 负责数据处理
        node.master:false
        node.ingest:true
        node.data:false
    Coordinate节点
        node.master:false
        node.ingest:false
        node.data:false
        
##### Hot && Warm Node Machine Learning Node

    GET /_cat/nodeattrs?v

    Hot Nodes 用于数据写入
    Warm Nodes 用于保存只读的索引，比较旧的数据

    配置 Hot & Warm Architecture
        标记节点(tagging)  
            node.attr.my_node_type: hot
            node.attr.my_node_type: warm
        配置索引到Hot Node
            PUT index
            {
                "settings":{
                    "number_of_shards":2,
                    "number_of_replicas":0,
                    "index.routing.allocation.require.my_node_type":"hot"
                }
            }
        
        配置索引到Warm 节点
            PUT index/_settings
            {
                "index.routing.allocation.require.my_node_type":"warm"
            }


> 分片（Primary Shard  & Replica Shard）

##### 主分片 提示系统存储容量

    用以解决数据水平扩展的问题，通过主分片，可以将数据分布到集群内的所有节点之上

    Primary Shard 可以将一份索引的数据 分散在多个Data Node上 实现存储的水平扩展
    主分片数在索引创建时候指定，后续默认不能修改，如要修改，需重建索引

    一个分片就是一个运行的lucene的实例
    主分片数在索引创建时指定，后续不允许修改，除非reindex

##### 副本 Replica Shard  用以解决数据高可用的问题。副本分片是主分片的拷贝

- [x]     数据可以用性


    通过引入副本分片提高数据的可用性。一旦主分片丢失，副本分片可以Promote成主分片。副本分片数可以动态调整，每个节点上都有完备的数据。
    
- [x]     提示系统的读取性能


    副本分片由主分片同步，通过支持增加Replica个数，可以提高读取的吞吐量
    
##### 分片的设定

    PUT /product
    {
        "setting":{
            "number_of_shards":2,
            "number_of_replicas":1
        }
    }

    容量规划 20-50G
    
##### 查看集群的健康状况

    GET _cluster/health

    Green   主分片与副本都正常分配 
    Yellow  主分片正常，副本分片未能正常分配
    Red     有主分片未能分配