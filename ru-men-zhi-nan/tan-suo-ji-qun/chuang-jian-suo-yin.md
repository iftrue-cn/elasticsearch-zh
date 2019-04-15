# 创建索引

现在让我们创建一个名为customer的索引：

```bash
curl -X PUT "localhost:9200/customer?pretty"
```

该命令使用PUT方式创建名为customer的索引。我们只需在调用的末尾追加pretty，让它漂亮地打印JSON响应\(如果有的话\)。

响应：

```javascript
{
  "acknowledged" : true,
  "shards_acknowledged" : true,
  "index" : "customer"
}
```

现在再次列出所有索引：

```bash
curl -X GET "localhost:9200/_cat/indices?v"
```

响应：

```text
health status index    uuid                   pri rep docs.count docs.deleted store.size pri.store.size
yellow open   customer veVgGi15SbyLiHiSTFAU4w   1   1          0            0       230b           230b
```

结果告诉我们，现在有一个名为customer的索引，它有一个主分片和一个副本\(默认值\)，其中没有包含任何文档。 您还可能注意到customer索引上标记了一个黄色的健康状态。回想一下前面的文章，黄色表示还没有分配一些副本。发生这种情况的原因是，在默认情况下，Elasticsearch为该索引创建了一个副本。由于我们目前只有一个节点在运行，所以直到稍后另一个节点加入集群时，才可以分配该副本\(用于高可用性\)。一旦将该副本分配到第二个节点，该索引的健康状态将变为绿色。

