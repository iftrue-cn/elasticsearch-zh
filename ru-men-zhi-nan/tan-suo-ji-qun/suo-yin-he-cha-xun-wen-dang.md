# 索引和查询文档

现在让我们把一些东西放入我们的customer索引中。我们将一个简单的customer文档索引到customer索引中，ID为1，如下所示：

```bash
curl -X PUT "localhost:9200/customer/_doc/1?pretty" -H 'Content-Type: application/json' -d'
{
  "name": "John Doe"
}
```

响应：

```text
{
  "_index" : "customer",
  "_type" : "_doc",
  "_id" : "1",
  "_version" : 1,
  "result" : "created",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 0,
  "_primary_term" : 1
}
```

从上面，我们可以看到在customer索引中成功创建了一个新的customer文档。文档的内部id也是1，这是我们在索引时指定的。

需要注意的是，Elasticsearch并不要求您在将文档编入索引之前先显式地创建索引。在前面的示例中，Elasticsearch将自动创建customer索引\(如果它之前不存在\)。

现在让我们检索刚才索引的文档：

```bash
curl -X GET "localhost:9200/customer/_doc/1?pretty"
```

响应：

```text
{
  "_index" : "customer",
  "_type" : "_doc",
  "_id" : "1",
  "_version" : 1,
  "_seq_no" : 0,
  "_primary_term" : 1,
  "found" : true,
  "_source" : {
    "name" : "John Doe"
  }
}
```

我们找到了一个ID为1的文档和另一个字段\_source，该字段返回上一步索引的完整JSON文档。

