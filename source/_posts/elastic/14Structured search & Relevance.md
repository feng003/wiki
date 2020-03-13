> Structured search 对结构化数据的搜索


    日期、布尔类型 和 数字都是结构化的

    Term查询、Prefix前缀查询
    

    POST book/_search
    {
      "query": {
        "constant_score": {
          "filter": {
            "range": {
              "price": {
                "gte": 10,
                "lte": 20
              }
            }
          },
          "boost": 1.2
        }
      }
    }


    POST book/_search
    {
      "query": {
        "constant_score": {
          "filter": {
            "range": {
              "created": {
                "gte": "now-11y"
              }
            }
          },
          "boost": 1.2
        }
      }
    }

****
> Relevance 相关性


    搜索的相关性算法 描述了一个文档 和 查询语句 匹配的程度 _score

    打分的本质就是 排序。es5之前使用 TF-IDF，现在采用BM25
    
##### 词频 TF term frequency

    检索词在一篇文档中出现的频率 = 次数 / 文档总字数

    度量一条查询 和 结果文档相关性的简单方法：将搜索中每一个词的TF进行相加

    Stop Word  "的" 不考虑TF
    
##### 逆文档频率 IDF Inverse Document Frequency 

    DF 检索词在 所有文档 中出现的频率

    log(全部文档数/检索词出现过的文档总数)
    本质上就是将TF求和 变成 加权求和

    相关性打分 
        q 查询语句
        d 匹配的文档

    score(q, d) = coord(q, d)* queryNorm(q) * (t in q 求和)(tf(t in d) * idf(t)^2*boost(t)*norm(t, d))
    
##### BM25

    bm25(d) = log(1+ (N-dft+0.5)/(dft+0.5))* ((ft,d) / (ft,d + k * (1 -bb + b* (1(d)/avgdl)) )

    k 默认值 1.2; b默认值是0.75(0-1)
    
    
    
**使用 explain 查看相关性算分**

    POST book/_search 
    {
      "explain": true, 
      "query": {
        "match_phrase": {
          "title": {
            "query": "数学"
          }
        }
      }
    }
    
##### Boosting Relevance 设置参数 影响查询结果算分
