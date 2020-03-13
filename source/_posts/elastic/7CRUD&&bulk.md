> 文档的 crud

名称 | 操作
---|---
Index   | PUT my_index/_doc/1{}
Create  | PUT my_index/create/1{}  POST my_index/_doc{}
Read    | GET my_index/_doc/1
Update  | POST my_index/_update/1{}
Delete  | DELETE my_index/_doc/1


    type 名 约定都用 _doc

    Create 如果ID已经存在，会失败
    
    Index 如果ID不存在，创建新的文档，否则，先删除现有的文档，再创建新的文档，版本会增加
    
    Update 文档必须已经存在，更新只会对相应字段做增量修改
    
    
> bulk API 批量操作


    一般建议是1000-5000个文档，大小建议是5-15MB，默认不能超过100M，可以在es的配置文件（即$ES_HOME下的config下的elasticsearch.yml）中，bulk的线程池配置是内核数+1。


    bulk批量操作的json格式解析
    bulk的格式：

    {action:{metadata}}\n
    {requstbody}\n (请求体)

    Index
    Create
    Update
    Delete
    
    POST _bulk
    { "index":{"_index":"book","_id":3} }
    { "isbn":"9787123456784","title":"test4" }
    { "create":{"_index":"book","_id":5}}
    { "isbn":"9787123456786","title":"test6"}
    { "update":{"_index":"book", "_id":3}}
    { "doc":{ "create":"2019-10-15" }}
    { "delete":{"_index":"book","_id":7}}
    
    
> mget 批量读取

    GET _mget
    {
      "docs":[
                {
                    "_index":"book",
                    "_id":1
                },
                {
                    "_index":"book",
                    "_id":2
                },
                 {
                    "_index":"book",
                    "_id":3
                }
            ]
    }
    
    
> msearch 批量查询

    POST book/_msearch
    {}
    {"query":{"match_all":{}},"size":10}
    
    
> 错误返回

问题 | 原因
---|---
无法连接     | 网络故障或集群挂了
连接无法关闭 | 网络故障或节点出错
429          | 集群过于繁忙
4XX          | 请求体格式有错
500          | 集群内部错误

    
> command 

    GET book/_doc/4

    POST book/_doc
    {
      "isbn":"9787123456783",
      "title":"test3"
    }
    
    PUT book/_create/1
    {
      "isbn":"9787123456781",
      "title":"test1"
    }
    
    PUT book/_doc/1?op_type=index
    {
      "isbn":"9787123456782",
      "title":"test2"
    }
    
    POST book/_update/1/
    {
      "doc":{
        "create":"2019-10-10"
      }
    }
    
    DELETE book/_doc/3