
    PUT ik_test

    GET ik_test/_search

    PUT /ik_test/_doc/1
    {
      "content":"美国留给伊拉克的是个烂摊子吗"
    }

    PUT /ik_test/_doc/2
    {
      "content":"公安部：各地校车将享最高路权"
    }

    PUT /ik_test/_doc/3
    {
      "content":"中韩渔警冲突调查：韩警平均每天扣1艘中国渔船"
    }

    PUT /ik_test/_doc/4
    {
      "content":"中国驻洛杉矶领事馆遭亚裔男子枪击 嫌犯已自首"
    }
    

    GET ik_test/_search
    {
       "query" : { "match" : { "content" : "中国" }},
        "highlight" : {
            "pre_tags" : ["<tag1>", "<tag2>"],
            "post_tags" : ["</tag1>", "</tag2>"],
            "fields" : {
                "content" : {}
            }
        }
    }
    