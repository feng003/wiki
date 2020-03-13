> mapping 模板

    PUT product
    {
      "mappings": {
          "properties": {
                "id": {
                    "type": "long"
                },
                "title": {
                    "type": "keyword"  //精确匹配
                },
                "content": {
                    "analyzer": "ik_max_word",
                    "search_analyzer": "ik_smart",
                    "type": "text", //全文检索
                    "fields": {
                        "keyword": {
                            "ignore_above": 256,
                            "type": "keyword"
                        }
                    }
                },
                "available": {
                    "type": "boolean"
                },
                "review": {
                    "type": "nested",
                    "properties": {
                        "nickname": {"type": "text"},
                        "text": {"type": "text"},
                        "stars": {"type": "integer"}
                    }
                },
                "publish_time": {
                    "type": "date",
                    "format": "yyyy-MM-dd HH:mm:ss||yyyy-MM-dd||epoch_millis"
                },
                "expected_attendees": {
                    "type": "integer_range"
                },
                "ip_addr": {
                    "type": "ip"
                },
                "suggest": {
                    "type": "completion"
                }
            }
        }
    }
    
