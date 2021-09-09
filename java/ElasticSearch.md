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
###### 聚合类型
`min,max,avg,sum,cardinality`
