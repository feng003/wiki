> Index Template

    帮助你设定mapping 和 setting，并按照一定的规则，自动匹配到新创建的索引之上
    
    GET book/_mapping

    PUT _template/template_default
    {
      "index_patterns":["*"],
      "order":0,
      "version":1,
      "settings":{
        "number_of_shards":1,
        "number_of_replicas":1
      }
    }
    
    
    PUT /_template/template_test
    {
      "index_patterns":["book*"],
      "order":1,
      "settings":{
        "number_of_shards":1,
        "number_of_replicas":2
      },
      "mappings":{
        "date_detection":false,
        "numeric_detection":true
      }
    }
    
    
    GET /_template/template_default
    GET /_template/temp*
    
> Dynamic Template 根据es识别的数据类型，结合字段名称，来动态设定字段类型

    所有的字符串类型都设定成keyword
    is 开头的设置成 boolean
    long_ 开头的是设置成 long
    
    PUT my_index
    {
      "mappings": {
        "dynamic_templates":[
          {
            "full_name":{
              "path_match":"name.*",
              "path_unmatch":"*.middle",
              "mapping":{
                "type":"text",
                "copy_to":"full_name"
              }
            }
          }]
      }
    }

    