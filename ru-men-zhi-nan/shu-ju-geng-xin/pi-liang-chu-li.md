# 批量处理

除了能够索引、更新和删除单个文档之外，Elasticsearch还提供了使用\_bulk API批量执行上述操作的能力。此功能非常重要，因为它提供了一种非常有效的机制，可以在尽可能少的网络往返的情况下尽可能快地执行多个操作。

一个简单的例子，下面的调用在一个批量操作中索引两个文档\(ID 1 - John Doe和ID 2 - Jane Doe\):

```bash
curl -X POST "localhost:9200/customer/_bulk?pretty" -H 'Content-Type: application/json' -d'
{"index":{"_id":"1"}}
{"name": "John Doe" }
{"index":{"_id":"2"}}
{"name": "Jane Doe" }
'
```

这个示例更新第一个文档\(ID为1\)，然后在一次批量操作中删除第二个文档\(ID为2\)：

```bash
curl -X POST "localhost:9200/customer/_bulk?pretty" -H 'Content-Type: application/json' -d'
{"update":{"_id":"1"}}
{"doc": { "name": "John Doe becomes Jane Doe" } }
{"delete":{"_id":"2"}}
'
```

注意，对于delete操作，它之后没有对应的源文档，因为删除操作只需要删除文档的ID。

bulk API不会因为某个操作失败而失败。如果一个操作由于某种原因失败，它将在失败之后继续处理其余的操作。当bulk API返回时，它将为每个操作提供一个状态\(与发送操作的顺序相同\)，以便检查某个特定操作是否失败。

