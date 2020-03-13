> 聚合


    1. Bucket Aggregation

    2. Metric Aggregation

    3. Pipeline Aggregation

    4. Matrix Aggregation


    POST /product/_search 
    {
      "size":0,
      "aggs":{
        "max_price":{
          "max": {
            "field": "price"
          }
        },
        "min_price":{
          "min":{
            "field": "price"
          }
        },
        "avg_price":{
          "avg":{
            "field": "price"
          }
        }
      }
    }


    POST /product/_search
    {
      "size": 0,
      "aggs": {
        "stats_price": {
          "stats": {
            "field": "price"
          }
        }
      }
    }


> Bucket & Metric


    Bucket

        Term & Range
        
        POST product/_search
        {
          "size": 0,
          "aggs": {
            "titles": {
              "terms": {
                "field": "title.keyword",
                "size": 10
              }
            }
          }
        }
        
        
        POST /product/_search 
        {
          "size": 0,
          "aggs": {
            "price_range": {
              "range": {
                "field": "price",
                "ranges": [
                  {"to":50},
                  {"from":50,"to":200},
                  {"from":100, "to":300}        ]
              }
            }
          }
        }
        
    Metric

        数学计算，输出一个值
            min/max/sun/avg/cardinality
        部分支持输出多个数值
            stats/percentiles/percentile_ranks