# 删除文档

删除文档相当简单。这个例子展示了如何删除ID为2的前一个客户：

```bash
curl -X DELETE "localhost:9200/customer/_doc/2?pretty"
```

请参阅\_delete\_by\_query API来删除与特定查询匹配的所有文档。值得注意的是，删除整个索引比使用delete By Query API删除所有文档要有效得多。

