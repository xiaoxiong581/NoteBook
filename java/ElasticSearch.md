##### 1. 范围查询
```json
{
  "query": {
    "range": {
      "price": {
        "gte": 0,
        "lte": 150
      }
    }
  },
  "from": 0,
  "size": 100,
  "sort": {
    "@timestamp": {
      "order": "desc"
    }
  }
}
```
##### 2. 对数据进行分组再聚合
```json
{
  "size": 0,
  "aggs": {
    "group_by_tags": {
      "range": {
        "field": "price",
        "ranges": [
          {
            "from": 0,
            "to": 100
          },
          {
            "from": 100,
            "to": 150
          }
        ]
      },
      "aggs": {
        "avg_price": {
          "avg": {
            "field": "price"
          }
        }
      }
    }
  }
}
```
```json
{
  "size": 0,
  "query": {
    "bool": {
      "must": [
        {
          "query_string": {
            "query": "A_5260011834700:*",
            "fields": [],
            "use_dis_max": true,
            "tie_breaker": 0,
            "default_operator": "or",
            "auto_generate_phrase_queries": false,
            "max_determined_states": 10000,
            "enable_position_increment": true,
            "fuzziness": "AUTO",
            "fuzzy_prefix_length": 0,
            "fuzzy_max_expansions": 50,
            "phrase_slop": 0,
            "escape": false,
            "split_on_whitespace": true,
            "boost": 1
          }
        },
        {
          "range": {
            "@timestamp": {
              "from": 1635936300000,
              "to": 1635937200000,
              "include_lower": true,
              "include_upper": true,
              "boost": 1
            }
          }
        }
      ],
      "disable_coord": false,
      "adjust_pure_negative": true,
      "boost": 1
    }
  },
  "sort": [
    {
      "@timestamp": {
        "order": "asc"
      }
    }
  ],
  "aggs": {
    "agg": {
      "terms": {
        "field": "A_5260011834700"
      }
    }
  }
}
```
###### 聚合类型
`min,max,avg,sum,cardinality`
