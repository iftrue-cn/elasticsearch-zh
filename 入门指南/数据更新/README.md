Elasticsearch提供接近实时的数据操作和搜索功能。默认情况下，从索引/更新/删除数据到它出现在搜索结果中，您可以期待一秒钟的延迟(刷新间隔)。这是与其他平台(如SQL)的一个重要区别，在SQL中，事务完成后数据立即可用。

### 索引/替换文档
我们之前已经了解了如何索引单个文档。让我们再回忆一下这个命令：
```bash
curl -X PUT "localhost:9200/customer/_doc/1?pretty" -H 'Content-Type: application/json' -d'
{
  "name": "John Doe"
}
'
```

同样，上面将把指定的文档索引到customer索引中，ID为1。如果我们用一个不同的(或相同的)文档再次执行上面的命令，Elasticsearch将替换(即重新索引)ID为1的现有文档：
```bash
curl -X PUT "localhost:9200/customer/_doc/1?pretty" -H 'Content-Type: application/json' -d'
{
  "name": "Jane Doe"
}
'
```

上面将ID为1的文档name从“John Doe”更改为“Jane Doe”。另一方面，如果我们使用不同的ID，则将索引一个新文档，并且索引中已有的文档保持不变。
```bash
curl -X PUT "localhost:9200/customer/_doc/2?pretty" -H 'Content-Type: application/json' -d'
{
  "name": "Jane Doe"
}
'
```
上面的索引是一个ID为2的新文档。

创建索引时，ID是可选的。如果没有指定，Elasticsearch将生成一个随机ID，然后使用它来索引文档。ElasticSearch生成的实际ID（或在前面的示例中显式指定的ID）作为索引API调用的一部分返回。

这个例子展示了如何索引一个没有显式ID的文档：
```bash
curl -X POST "localhost:9200/customer/_doc?pretty" -H 'Content-Type: application/json' -d'
{
  "name": "Jane Doe"
}
'
```

> 在上面的例子中，我们使用POST谓词而不是PUT，因为我们没有指定ID。
