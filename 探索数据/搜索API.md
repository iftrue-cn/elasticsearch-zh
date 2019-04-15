现在让我们从一些简单的搜索开始。运行搜索有两种基本方法：一种是通过REST请求URI发送搜索参数，另一种是通过REST请求体发送搜索参数。请求体方法允许您更富表现力，还可以以更可读的JSON格式定义搜索。我们将尝试请求URI方法的一个示例，但是在本教程的其余部分中，我们将只使用请求体方法。

用于搜索的REST API可以使用_search访问。这个例子返回索引库中的所有文档：
```bash
curl -X GET "localhost:9200/bank/_search?q=*&sort=account_number:asc&pretty"
```

让我们首先分析一下搜索调用。我们正在索引库中搜索(_search)， q=*参数指示Elasticsearch匹配索引中的所有文档。sort=account_number:asc参数指示使用每个文档的account_number字段按升序对结果排序。同样，pretty参数只告诉Elasticsearch返回打印得很漂亮的JSON结果。

响应（部分）：
```
{
  "took" : 63,
  "timed_out" : false,
  "_shards" : {
    "total" : 5,
    "successful" : 5,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
        "value": 1000,
        "relation": "eq"
    },
    "max_score" : null,
    "hits" : [ {
      "_index" : "bank",
      "_type" : "_doc",
      "_id" : "0",
      "sort": [0],
      "_score" : null,
      "_source" : {"account_number":0,"balance":16623,"firstname":"Bradshaw","lastname":"Mckenzie","age":29,"gender":"F","address":"244 Columbus Place","employer":"Euron","email":"bradshawmckenzie@euron.com","city":"Hobucken","state":"CO"}
    }, {
      "_index" : "bank",
      "_type" : "_doc",
      "_id" : "1",
      "sort": [1],
      "_score" : null,
      "_source" : {"account_number":1,"balance":39225,"firstname":"Amber","lastname":"Duke","age":32,"gender":"M","address":"880 Holmes Lane","employer":"Pyrami","email":"amberduke@pyrami.com","city":"Brogan","state":"IL"}
    }, ...
    ]
  }
}
```

在响应中，我们看到以下部分：

* took – Elasticsearch执行搜索所用的时间(以毫秒为单位)
* timed_out – 搜索是否超时
* _shards – 搜索了多少分片，以及搜索成功/失败的分片的数量
* hits – 搜索结果
* hits.total – 包含与搜索条件匹配的文档总数信息的对象
   * hits.total.value - 总命中数 (由 hits.total.relation 解释).
   * hits.total.relation - hits.total.value是命中数，它大于或等于总命中数的下限，在这种情况下，它是gte。
* hits.hits – 搜索结果的实际数组（默认为前10个文档）
* hits.sort - 排序字段 (如果按score排序则缺少该项)
* hits._score and max_score - 暂时忽略这些字段

hits.total的准确性由请求参数track_total_hits控制，当设置为true时，请求将精确地跟踪总命中(“relationship”:“eq”)。默认值为10,000，这意味着总命中数可以精确地跟踪到10,000个文档。通过显式地将track_total_hits设置为true，可以强制进行准确的计数。

这里是与上面搜索相同的替代请求体方法：
```bash
curl -X GET "localhost:9200/bank/_search" -H 'Content-Type: application/json' -d'
{
  "query": { "match_all": {} },
  "sort": [
    { "account_number": "asc" }
  ]
}
'
```

这里的区别在于，我们没有在URI中传递q=*，而是向SearchAPI提供一个JSON格式的查询请求体。我们将在下一章中讨论这个JSON查询。

需要理解的重点是，一旦您返回搜索结果，ElasticSearch将完全完成请求，并且不会维护任何类型的服务器端资源或在结果中打开光标。这与许多其他平台（如SQL）形成了鲜明的对比，在这些平台中，您最初可能会提前获得查询结果的一部分子集，然后通过光标或翻页的方式继续获取数据，但是在此必须要重新请求服务器。