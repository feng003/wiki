> 同一台机器上  network.host 全部配置同一IP

##### 主节点

    cluster.name: my-application

    node.name: master
    node.master: true

    network.host: 127.0.0.1

    http.port: 9200
    http.cors.enabled: true
    http.cors.allow-origin: "*"

##### 从节点slave1

    cluster.name: my-application

    node.name: node1

    network.host: 127.0.0.1

    http.port: 9201

    #发现主节点(通过主节点的ip)
    discovery.zen.ping.unicast.hosts: ["127.0.0.1"]
    
##### 从节点slave2

    cluster.name: my-application

    node.name: node2

    network.host: 127.0.0.1

    http.port: 9202

    #发现主节点(通过主节点的ip)
    discovery.zen.ping.unicast.hosts: ["127.0.0.1"]


    1）master node——master节点点主要用于元数据(metadata)的处理，比如索引的新增、删除、分片分配等。

    2）data node——data 节点上保存了数据分片。它负责数据相关操作，比如分片的 CRUD，以及搜索和整合操作。这些操作都比较消耗 CPU、内存和 I/O 资源；

    3）client node——client 节点起到路由请求的作用，实际上可以看做负载均衡器。


    #分片数和副本数
    index.number_of_shards: 5
    index.number_of_replicas: 1

    #master选举最少的节点数，这个一定要设置为N/2+1，其中N是：具有master资格的节点的数量，而不是整个集群节点个数
    discovery.zen.mininum_master_nodes: 2

    #discovery ping的超时时间，拥塞网络，网络状态不佳的情况下设置高一点
    discovery.zen.ping.timeout: 3s

    #关闭自动发现节点
    discovery.zen.ping.multicast.enabled: false

    #定义发现的节点
    discovery.zen.ping.unicast.hosts: ["192.168.1.1:9201", "192.168.1.1:9202"]

[elasticsearch 分布式集群搭建](https://www.cnblogs.com/xuwenjin/p/9018006.html)