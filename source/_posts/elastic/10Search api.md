# search api

> URI search
    
    
    在url中使用 查询参数

    使用 "q" 指定查询字符串  "query string syntax" KV键值对

    df 默认字段 不指定时，会对所有字段进行查询

    sort 排序 form size 分页

    profile 查看查询是如何被执行的

    TermQuery   Beautiful Mind == Beautiful or Mind
    PhraseQuery "Beautiful Mind" == Beautiful and Mind
    
1.     布尔操作 AND 、OR、NOT 或者 / || / !
2.     分组 () + must - must_not
3.     范围查询 []闭区间  {}开区间 year:{2018 TO 2019}
4.     算数查询 year:>2000 year:(>2010 && <=2018)
5.     通配符查询 ? 代表一个字符 * 代表0或多个字符 title:mi?d   title:be*
6.     正则表达 title:[bt]oy
7.     模糊匹配与近似查询 title:befautifl~1


##### demo 

    curl -XGET
    "http://localhost:9200/book/_search?q=test&df=title&sort=create:desc&from=1&size=10&timeout=1s"
    {
        "profile":true
    }

    GET /book/_search?q=test&df=title
    {
      "profile": "true"
    }

    //使用引号 phrase查询
    GET /movies/_search?q=title:"Beautiful Mind"
    {
      "profile": "true"
    }
    
    //泛查询
    GET /movies/_search?q=title:Beautiful Mind
    {
      "profile": "true"
    }
    
    //分组 bool 查询
    GET /movies/_search?q=title:(Beautiful Mind)
    {
      "profile": "true"
    }

> Request Body Search

    基于JSON
    Query Domain Specific Language (DSL)
    
##### 脚本字段 汇率转换


##### 使用查询表达式 - Match


##### 短语搜索 Match Phrase


##### Query string query

    POST book/_search
    {
      "query": {
        "query_string": {
          "default_field": "title",
          "query": "this AND that OR thus"
        }
      }
    }


##### Simple Query string query

    POST book/_search
    {
      "query": {
        "simple_query_string": {
          "query": "",
          "fields": ["title"],
          "default_operator": "AND"
        }
      }
    }
    
    
##### demo


    POST /movies,404_idx/_search?ignore_unavailable=true
    {
      "from":10,
      "size": 20, 
      "sort": [{"year": "desc"}],
      "_source": ["title","id","year"], 
      "profile": "true",
      "query": {
        "match_all": {}
      }
    }
    
    POST movies/_search
    {
      "query": {
        "match": {
          "title": "Last Christmas"
        }
      }
    }
    
    POST movies/_search
    {
      "query": {
        "match": {
          "title": {
            "query":  "Last Christmas",
            "operator": "and"
          }
        }
      }
    }
    
    POST movies/_search 
    {
      "query": {
        "match_phrase": {
          "title": {
            "query": "one love"
          }
        }
      }
    }
    
    POST movies/_search 
    {
      "query": {
        "match_phrase": {
          "title": {
            "query": "one love",
            "slop": 1
          }
        }
      }
    }
    

语法 | 范围
---|---
/_search        | 集群上所有的索引
/index1/search  | index1
/index1,index2/_search | index1 index2
/index*/_search | 以index开头的索引


    Information Retrieval 衡量相关性
    
    Precision (查准率)
    Recall (查全率)
    Ranking 按照相关度进行排序
