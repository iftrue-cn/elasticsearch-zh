# 执行聚合

聚合提供了对数据进行分组和提取统计信息的能力。理解聚合最简单的方法是大致将其等同于SQL语句中的 GROUP by和SQL聚合函数。在Elasticsearch中，您可以执行返回命中的搜索，同时在一个响应中返回与所有命中分离的聚合结果。这是非常强大和高效的，因为您可以运行查询和多个聚合，并一次性获得这两个\(或任何一个\)操作的结果，从而避免使用简洁和简化的API进行网络往返。

首先，这个示例按状态对所有帐户进行分组，然后返回按count降序排列的前10个\(默认\)状态\(也是默认\)：

```bash
curl -X GET "localhost:9200/bank/_search" -H 'Content-Type: application/json' -d'
{
  "size": 0,
  "aggs": {
    "group_by_state": {
      "terms": {
        "field": "state.keyword"
      }
    }
  }
}
'
```

上述聚合在概念上与以下SQL语句类似：

```sql
SELECT state, COUNT(*) FROM bank GROUP BY state ORDER BY COUNT(*) DESC LIMIT 10;
```

响应（部分）：

```text
{
  "took": 29,
  "timed_out": false,
  "_shards": {
    "total": 5,
    "successful": 5,
    "skipped" : 0,
    "failed": 0
  },
  "hits" : {
     "total" : {
        "value": 1000,
        "relation": "eq"
     },
    "max_score" : null,
    "hits" : [ ]
  },
  "aggregations" : {
    "group_by_state" : {
      "doc_count_error_upper_bound": 20,
      "sum_other_doc_count": 770,
      "buckets" : [ {
        "key" : "ID",
        "doc_count" : 27
      }, {
        "key" : "TX",
        "doc_count" : 27
      }, {
        "key" : "AL",
        "doc_count" : 25
      }, {
        "key" : "MD",
        "doc_count" : 25
      }, {
        "key" : "TN",
        "doc_count" : 23
      }, {
        "key" : "MA",
        "doc_count" : 21
      }, {
        "key" : "NC",
        "doc_count" : 21
      }, {
        "key" : "ND",
        "doc_count" : 21
      }, {
        "key" : "ME",
        "doc_count" : 20
      }, {
        "key" : "MO",
        "doc_count" : 20
      } ]
    }
  }
}
```

我们可以看到在ID\(Idaho 爱达荷州\)有27个帐户，其次是TX\(Texas 德克萨斯州\)的27个帐户，然后是AL\(Alabama 阿拉巴马州\)的25个帐户，依此类推。

注意，我们将size=0设置为不显示搜索结果，因为我们只想看到响应中的聚合结果。

基于前面的汇总，本示例按状态计算平均帐户余额\(同样只针对按count降序排列的前10个状态\)：

```bash
curl -X GET "localhost:9200/bank/_search" -H 'Content-Type: application/json' -d'
{
  "size": 0,
  "aggs": {
    "group_by_state": {
      "terms": {
        "field": "state.keyword"
      },
      "aggs": {
        "average_balance": {
          "avg": {
            "field": "balance"
          }
        }
      }
    }
  }
}
'
```

注意，我们如何将average\_balance聚合嵌套在group\_by\_state聚合中。这是所有聚合的常见模式。聚合中可以任意嵌套聚合。

基于之前的聚合，我们现在按降序对平均余额排序：

```bash
curl -X GET "localhost:9200/bank/_search" -H 'Content-Type: application/json' -d'
{
  "size": 0,
  "aggs": {
    "group_by_state": {
      "terms": {
        "field": "state.keyword",
        "order": {
          "average_balance": "desc"
        }
      },
      "aggs": {
        "average_balance": {
          "avg": {
            "field": "balance"
          }
        }
      }
    }
  }
}
'
```

这个例子展示了我们如何按照年龄等级\(20-29岁，30-39岁，40-49岁\)分组，然后按性别分组，最后得到每个年龄等级，每个性别的平均账户余额：

```bash
curl -X GET "localhost:9200/bank/_search" -H 'Content-Type: application/json' -d'
{
  "size": 0,
  "aggs": {
    "group_by_age": {
      "range": {
        "field": "age",
        "ranges": [
          {
            "from": 20,
            "to": 30
          },
          {
            "from": 30,
            "to": 40
          },
          {
            "from": 40,
            "to": 50
          }
        ]
      },
      "aggs": {
        "group_by_gender": {
          "terms": {
            "field": "gender.keyword"
          },
          "aggs": {
            "average_balance": {
              "avg": {
                "field": "balance"
              }
            }
          }
        }
      }
    }
  }
}
'
```

还有许多其他聚合功能，我们在这里不会详细讨论。如果您想做进一步的实验，那么聚合参考指南是一个很好的起点。

