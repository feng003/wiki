> Suggester as type


    原理：将输入的文本分解为Token，然后在索引的字典里查找相似的Term并返回


    四种类别的Suggesters

        Term & Phrase Suggester
        Complete & Context Suggester
        
> Term Suggester


    suggestion mode

        missing 索引中存在，就不提供建议

        popular 推荐出现频率更加高的词
        
        always 无论是否存在，都提供建议

    POST /book/_search
    {
      "size": 5,
      "query": {
        "match": {
          "title": "书学"
        }
      },
      "suggest": {
        "term-sugestion": {
          "text": "书学",
          "term": {
            "suggest_mode":"missing",
            "field": "title",
            "prefix_length":0,
            "sort":"frequency"
          }
        }
      }
    }
    
> Phrase Suggester


    params
    
        Suggest Mode : missing,popular,always
        Max Errors : 最多可以拼错的Terms数
        Confidence ：限制返回结果数，默认为 1
        

    POST /book/_search
    {
      "size": 10,
      "query": {
        "match": {
          "clc": "英语"
        }
      },
      "suggest": {
        "my-suggestion": {
          "text": "英语",
          "phrase":{
            "field":"clc",
            "max_errors":2,
            "confidence":0,
            "direct_generator":[{
              "field":"clc",
              "suggest_mode":"always"
            }],
            "highlight":{
              "pre_tag":"<em>",
              "post_tag":"</em>"
            }
          }
        }
      }
    }
    
> The Completion Suggester  Auto Complete


> 精准度和召回率


    精准度 Completion > Phrase > Term
    召回率 Term > Phrase > Completion
    性能   Completion > Phrase > Term

    
    