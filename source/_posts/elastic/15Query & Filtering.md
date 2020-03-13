> Query Context & Filter Context


    Query Context 相关性算分

    Filter Context 不需要算分，可利用Cache 获得更好的性能
    
> bool 条件组合查询


    一个bool查询，是一个 或 多个查询子句的组合


title | content
---|---
must     | Query Context  必须匹配，贡献算分
should   | Query Context  选择性匹配，贡献算分
must_not | Filter Context 查询子句，必须不能匹配
filter   | Filter Context 不贡献算分，必须匹配


> 控制查询的精确度

##### boosting & boosting query 增加权重 

****

> 单字符串多字段查询 Disjunction Max Query


    POST book/_search
    {
      "explain": true,
      "query": {
        "bool": {
          "should": [
            {"match": {"title": "绘本"}},
            {"match": {"tag": {
                "query":"绘本",
                "boost":2
                }
              }
            }
          ]
        }
      }
    }

    title 和 tag 相互竞争，不应该将分数简单叠加，而是应该找到单个最佳匹配的字段的评分
    
##### Disjunction Max Query

    将任何与任一查询匹配的文档作为结果返回，采用字段上最匹配的评分最终评分返回

    POST book/_search
    {
      "explain": true,
      "query": {
        "dis_max": {
          "queries": [
            {"match": {"title": "儿童 绘本"}},
            {"match": {"tag": "儿童 绘本"}}
          ]
        }
      }
    }
    
> 单字符串多字段查询 Multi Match

##### 最佳字段(best fields)

    当字段之间相互竞争，又相互关联，评分来自最匹配字段
    
    POST book/_search
    {
      "query": {
        "multi_match": {
          "query": "儿童 绘本",
          "type": "best_fields", 
          "fields": ["title","tag","clc"],
          "tie_breaker": 0.2,
          "minimum_should_match": "50%"
        }
      }
    }

##### 多数字段(most fields)
        
    "type": "most_fields", 

##### 混合字段(cross fields)

    地址、人名 搜索

    "type": "cross_fields",
    "operator":"and"