> 基于Term 的查询


    Term是表达语意的最小单位。搜索和利用统计语言模型进行自然语言处理都需要Term

    Term Level Query: Term Query/Range Query/Exists Query/Prefix Query/WildCard Query

    ES中 Term查询 对输入不做 分词。会将输入作为一个整体，在倒排索引中查找准确的词项，并且使用相关度算分公式 为每个包含该词项的文档进行 相关度算分 

    可以通过Constant Score 将查询转换成一个 Filtering，避免算分，并利用缓存。


    POST book/_search
    {
      "profile": "true", 
      "query": {
        "term": {
          "clc": {
            "value": "长篇小说\r"
          }
        }
      }
    }

    POST book/_search
    {
      "profile": "true", 
      "query": {
        "term": {
          "clc.keyword": {
            "value": "长篇小说\r"
          }
        }
      }
    }
        

> 多字段 Mapping 和 Term 查询


    不会做 分词 处理

    POST book/_search
    {
      "profile": "true", 
      "query": {
        "constant_score": {
          "filter": {
            "term": {
              "clc": "绘本"
            }
          },
          "boost": 1.2
        }
      }
    }


> 基于全文的查询


    match query / march phrase query / query string query

    索引和搜索时都会进行分词，查询字符串先传递到一个合适的分词器，然后生成一个供查询的词项列表

    查询时候，先会对输入的查询进行分词，然后每个词项逐个进行底层的查询，最终将结果进行合并。并为每个文档生产一个算分。
    

```
graph LR
A[martrix reloaded]-->B(matrix)
A-->C(reloaded)
B-->D(根据title的倒排索引查询并打分)
C-->D
D-->E(汇总得分)
E-->F(按照得分排序返回结果)
```