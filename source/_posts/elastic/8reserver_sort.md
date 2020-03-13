
ID | Content
---|---
1 | master es
2 | es server
3 | es logstash



Term | Count | DocumentId:Postion
---|--- |---
es          | 3 | 1:1 2:0 3:0
master      | 1 | 1:0
server      | 1 | 2:0
logstash    | 1 | 3:0

> 倒排索引的核心组成

    单词词典(Term Dictionary) 记录所有文档的单词，记录单词到倒排列表的关联关系
    
    倒排列表(Posting List) 记录了单词对应的文档集合，由倒排索引项组成
    
        倒排索引项(Posting)
        
        文档ID
        词频TF - 用于相关性评分
        位置Postion - 用于语句搜索
        偏移Offset - 实现高亮显示
       
       
ID | Content
---|---
1 | master es
2 | es server
3 | es logstash

##### es 这个词的倒排索引项

Doc ID | TF | Position | Offset
---|--- |--- |---
1 | 1 | 1 | <7,10>
2 | 1 | 0 | <0,3>
3 | 1 | 0 | <0,3>

> es 的倒排索引

    es的json文档中的每个字段，都有自己的倒排索引
    
    可以指定对某些字段不做索引