# 更新文档

除了能够索引和替换文档外，我们还可以更新文档。请注意，Elasticsearch实际上并没有在底层执行就地更新。每当我们执行更新时，Elasticsearch都会删除旧文档，然后一次性为新文档添加更新索引。

这个例子展示了如何通过将name字段更改为“Jane Doe”来更新我们之前的文档\(ID为1\)：

```bash
curl -X POST "localhost:9200/customer/_update/1?pretty" -H 'Content-Type: application/json' -d'
{
  "doc": { "name": "Jane Doe" }
}
'
```

这个例子展示了如何通过将name字段更改为“Jane Doe”来更新我们之前的文档\(ID为1\)，同时添加一个age字段：

```bash
curl -X POST "localhost:9200/customer/_update/1?pretty" -H 'Content-Type: application/json' -d'
{
  "doc": { "name": "Jane Doe", "age": 20 }
}
'
```

还可以使用简单的脚本执行更新。这个例子使用脚本将年龄增加5岁：

```bash
curl -X POST "localhost:9200/customer/_update/1?pretty" -H 'Content-Type: application/json' -d'
{
  "script" : "ctx._source.age += 5"
}
'
```

在上面的例子中，ctx.\_source引用将要更新的当前源文档。

Elasticsearch提供了更新给定查询条件的多个文档的能力\(比如SQL update - where语句\)，后面文章有详细介绍。

