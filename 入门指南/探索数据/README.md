### 数据集示例
现在我们已经了解了基本知识，让我们尝试使用更真实的数据集。我准备了一个虚构的JSON客户银行账户信息文档示例。每个文档都有以下结构：
```json
{
    "account_number": 0,
    "balance": 16623,
    "firstname": "Bradshaw",
    "lastname": "Mckenzie",
    "age": 29,
    "gender": "F",
    "address": "244 Columbus Place",
    "employer": "Euron",
    "email": "bradshawmckenzie@euron.com",
    "city": "Hobucken",
    "state": "CO"
}
```

该数据是使用<a href="http://www.json- generator.com" target="_blank">www.json- generator.com</a>生成的，因此请忽略数据的实际值和语义，因为它们都是随机生成的。

### 加载示例数据集
您可以从这里下载<a href="https://raw.githubusercontent.com/elastic/elasticsearch/master/docs/src/test/resources/accounts.json" target="_blank">示例数据集(accounts.json)</a>。将其放到当前目录，并将其加载到集群中，如下所示：
```bash
curl -H "Content-Type: application/json" -XPOST "localhost:9200/bank/_bulk?pretty&refresh" --data-binary "@accounts.json"
curl "localhost:9200/_cat/indices?v"
```

响应：
```
health status index uuid                   pri rep docs.count docs.deleted store.size pri.store.size
yellow open   bank  l7sSYV2cQXmu6_4rJWVIww   5   1       1000            0    128.6kb        128.6kb
```

这意味着我们刚刚成功地将索引的1000个文档批量存储到索引库中。