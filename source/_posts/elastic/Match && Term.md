#### Match 在匹配时会对所查找的关键词进行分词，然后按分词匹配查找；Term 会直接对关键词进行查找。 一般模糊查找的时候，多用match；精确查找时可用term。


    //匹配 my cat 任一均可
    {  
        "match": { "title": "my cat"}  
    }

    {  
      "bool": {  
        "should": [  
          { "term": { "title": "my" }},  
          { "term": { "title": "cat"   }}  
        ]  
      }  
    }
    // 精确匹配 "my cat"
    {
      "bool": {
        "should": [
          { "term": { "title": "my cat" }}  
        ]  
      }
    }