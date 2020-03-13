> analysis and analyzer

    analysis 分词/文本分析 把全文本转换一系列单词( term/token )的过程
    
    analysis 是通过analyzer 来实现的
    
    除了在数据写入时转换词条，匹配query语句时候也需要用相同的分析器对查询语句进行分析
    
> analyzer


```
graph LR
Character-Filters-->Tokenizer
Tokenizer-->Token-Filters
```

    Character Filters 针对原始文本处理 例如去除html

    Tokenizer 按照规则切分为单词
    
    Token Filter 将切分的单词进行加工、小写、删除stopwords、增加同义词
    
> es 内置分词器

    Standard Analyzer 默认 按词切分 小写处理
    Simple Analyzer 按非字母切分(符号被过滤) 小写处理
    Stop Analyzer 停用词过滤(the a is) 小写处理
    Whitespace Analyzer 按空格切分 不转小写
    Keyword Analyzer 不分词 直接将输入当输出
    Patter Analyzer 正则表达式 默认 \W+(非字符分隔)
    Language 常见语言的分词器
    Customer Analyzer 自定义分词器
    
> 使用 _analyzer API

    GET /_analyze
    {
      "analyzer": "standard",
      "text":"我是中国人，我爱中国"
    }
    
    POST book/_analyze
    {
      "field": "title",
      "text":"test"
    }
    
    POST /_analyze
    {
      "tokenizer": "standard",
      "filter": ["lowercase"],
      "text": "Mastre es"
    }
    
> Standard Analyzer 默认 按词切分 小写处理

    GET /_analyze
    {
      "analyzer": "standard",
      "text":"I am zhangbin I am chinese，我爱中国"
    }
    
>  Simple Analyzer 按非字母切分(符号被过滤) 小写处理

    GET /_analyze
    {
      "analyzer": "simple",
      "text":"I am zhangbin I am chinese，Fox-12,我爱中国"
    }
    
> Whitespace Analyzer 按空格切分 不转小写

    GET /_analyze
    {
    "analyzer": "whitespace",
    "text":"I am zhangbin I am chinese，Fox-12,我爱中国"
    }

> Stop Analyzer 停用词过滤(the a is) 小写处理

> Language 常见语言的分词器

    GET /_analyze
    {
    "analyzer": "icu_analyzer",
    "text":"I am zhangbin I am chinese，Fox-12,我爱中国"
    }
    
> 中文分词器 icu  ik


    ./elasticsearch-plugin install analysis-icu

    wget https://artifacts.elastic.co/downloads/elasticsearch-plugins/analysis-icu/analysis-icu-7.4.2.zip

    wget https://github.com/medcl/elasticsearch-analysis-pinyin/archive/v7.4.2.tar.gz

    wget https://github.com/medcl/elasticsearch-analysis-ik/archive/v7.4.2.tar.gz
    
[lk](https://github.com/medcl/elasticsearch-analysis-ik)

[pinyin](https://github.com/medcl/elasticsearch-analysis-pinyin)

[hanlp](https://github.com/KennFalcon/elasticsearch-analysis-hanlp/tree/master)

[thulac](https://github.com/microbun/elasticsearch-thulac-plugin)
