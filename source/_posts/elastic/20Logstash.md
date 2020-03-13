> Logstash 数据搜索处理引擎


```
graph LR
B[文件 日志 filebeats]-->A(Logstash)
C[Http]-->A
D[sql]-->A
E[kafka]-->A
A-->G{Es}
A-->F{analywsis mongodb}
A-->H{Archiving s3}
A-->I{Alerting email}
```

##### Logstash Concepts

    Pipeline
        input-filter-output
        插件生命周期管理
        队列管理
        
    Logstash Event 数据在内部流转时的具体表现形式
        数据在input阶段被转换为Event，在output被转化成目标格式数据


```
graph LR
input--> |Event| A(Filter)
A--> |Event| output
```

数据采集 | 数据解析 | 数据输出
--- |--- |---
Stdin | Mutate      | Es
JDBC  | Date        |
无    | user agent  |

##### Logstash 配置文件

    bin/logstash -f logstash.conf

    input {
      // stdin{}
      file {
        path => "/home/ubuntu/tmp/product.csv"
        start_position => "beginning"
        sincedb_path => "/dev/null"
      }
    }
    filter {
      // grok { match => {} }
      // date { match => [] }
      csv {
        separator => ","
        columns => ["id","isbn","title","author","price","rating","tag","clc","created"]
      }
      mutate {
          split => {"clc"=>"-"}
          remove_field=>["path","host","@timestamp","message"]
      }
      
      mutate {
          convert => {
              "year"=>"integer"
          }
          strip => ["title"]
          remove_field=>["path","host","@timestamp","message"]
      }
    }
    output {
       elasticsearch {
         hosts => "http://localhost:9200"
         index => "book"
         document_id => "%{id}"
       }
      stdout {}
    }

##### Filter Plugin 对 logstash event 进行处理，例如 解析、删除字段、类型转换

    Date    日期 
    Dissect 分隔符
    Grok    正则匹配
    Mutate  处理字段 重命名、删除、替换
    Ruby
    

