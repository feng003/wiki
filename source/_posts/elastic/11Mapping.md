> Mapping

##### Mapping 类似数据库中的schema的定义

    定义索引中的字段的名称
    自定义字段的数据类型 例如 字符串、数字、布尔
    字段，倒排索引的相关配置
    
##### Mapping 会把JSON文档映射成lucene所需要的扁平格式

##### 一个Mapping 属于一个索引的Type

    每个文档都属于一个Type
    一个Type有一个Mapping定义
    
    
##### 字段的数据类型

##### 简单类型

    Text/Keyword
    Date
    Integer/Floating
    Boolean
    IPv4 & IPv6
    
##### 复杂类型 对象和嵌套类型

##### 特殊类型

    geo_point geo_shape percolator
    
**** 

> Dynamic Mapping

    写入文档时候，如果索引不存在，会自动创建索引
    
    查看 Mapping  GET movies/_mappings
    
    

JSON类型 | es类型 2
---|---
字符串 | 日期 Date；数字 float；Text 增加keyword子字段
布尔值 | boolean
浮点数 | float
整数   | long
对象   | Object
数组   | 由第一个非空数值的类型所决定
空值   | 忽略


    PUT book/_doc/3
    {
      "title":"this is title",
      "isbn":9782146321455,
      "status":false,
      "code":1
    }
    
     GET book/_mappings
     
##### 能否更改Mapping的字段类型

    新增字段 
    
        dynamic 设为true   有新增字段的文档写入，Mapping也同时被更新
        dynamic 设为false  Mapping不会被更新，新增字段的数据无法被索引，单信息会出现在 _source 中
        dynamic 设为strict 文档写入失败 
    
    已有字段 不支持修改字段定义
    
    如果系统改变字段类型，必须Reindex API 重建索引
  
  
##### 控制 dynamic mappings


 title | true  | false  | strict
--- |--- |--- |---
文档可索引    | y | y | n
字段可索引    | y | n | n
mapping被更新 | y | n | n


    PUT book/_mapping 
    {
      "dynamic":false
    }

    PUT book/_doc/6
    {
      "title":"this is title",
      "isbn":9782146321455,
      "status":false,
      "code":1,
      "desc":"this is desc"
    }
    
    POST book/_search
    {
      "profile": "true", 
      "query": {
        "match": {
          "desc": "this is desc"
        }
      }
    }
    
    GET book/_mapping
    
****

> 显示Mapping设置

1.     创建一个临时的index 写入一些样本数据
2.     通过访问Mapping API获得该临时文件的动态Mapping定义
3.     修改后，使用该配置创建你的索引
4.     删除临时索引
    
##### 控制当前字段是否被索引

    Index - 控制当前字段是否被索引，默认为true。设置成false 该字段不可被搜索
    
##### Index Options 控制倒排索引记录的内容

    四种不同级别的Index Options 配置

    1. docs 记录doc id
    2. freqs 记录doc id 和 term frequencies
    3. positions 记录doc id、term frequencies、term position
    4. offsets doc id、term frequencies、term position、character offects
    
    Text 类型默认记录 positions 其他默认 docs
    

##### null_value 

    
    需要对null值实现搜索
    
    只有keyword类型支持设定null_value
    
##### copy_to 设置 将字段的数值拷贝到目标字段


    copy_to 的目标字段不出现在_source中
    
##### 数组类型

**** 

> 多字段类型 Mappings


##### 多字段特性

    厂商名字实现精确匹配
    
        增加一个keyword字段
        
    使用不同的analyzer
    
        不同语言
        pinyin字段的搜索
        支持为搜索和索引制定不同的analyzer
        
##### Exact values VS Full Text

    精确值 Exact value 不需要被分词；包括数字 日期 具体的一个字符串 
        
        es 中的 keyword
        Exact value 在索引时，不需要做特殊的分词处理
        
    全文本 非结构化的文本数据
        
        es 中的 text
        
##### 自定义分词

 **Character Filters**   
    
    1.在tokenizer 之前对文本进行处理，例如增加删除以及替换字符。可以配置多个 character filters。会影响 tokenizer的 position 和 offset 信息
    
    2.自带的character filters
    
    html strip 去除html标签
    mapping    字符串替换
    pattern replace 正则匹配替换
    
    POST _analyze
    {
      "tokenizer": "keyword",
      "char_filter": ["html_strip"],
      "text":"<span>elk</span>"
    }
    
    POST _analyze
    {
      "tokenizer": "standard",
      "char_filter": [
        {
          "type":"mapping",
          "mappings":["- => _"]
        }
        ],
      "text":"123-456.i-123"
    }
    
    POST _analyze
    {
      "tokenizer": "standard",
      "char_filter": [
        {
          "type":"pattern_replace",
          "pattern":"http://(.*)",
          "replacement":"$1"
        }],
        "text":"http://www.baidu.com"
    }
    
    
**Tokenizer**    
    
    1. 将原始的文本按照一定的规则，切分为词(term or token)
    2. es 内置的 tokenizers
        
        whitespace/standard/uax_url_email/pattern/keyword/path hierarchy
        
    3. 开发插件
    
    POST _analyze
    {
      "tokenizer": "path_hierarchy",
      "text":"/user/java/tomcat/a/b/"
    }
    

**Token Filters**   

    1. 将tokenizer 输出的单词(term) 进行增加、修改、删除
    2. 自带的token filters
    
        lowercase/stop/synonym (添加近义词)
        
    GET _analyze
    {
      "tokenizer": "whitespace",
      "filter": ["stop"],
      "text":["the rain in spain falls mainly on the plain"]
    }

##### config

    PUT my_index
    {
      "settings": {
        "analysis": {
          "analyzer": {
            "my_customer_analyzer":{
              "type":"custom",
              "char_filter":["emoticons"],
              "tokenizer":"punctuation",
              "filter":[
                "lowercase",
                "english_stop"
              ]
            }
          },
          "tokenizer": {
            "punctuation":{
              "type":"pattern",
              "pattern":"[.,!?]"
            }
          },
          "char_filter": {
            "emoticons":{
              "type":"mapping",
              "mappings":[]
            }
          },
          "filter": {
            "english_stop":{
              "type":"stop",
              "stopwords":"_english_"
            }
          }
        }
      }
    }
        