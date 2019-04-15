# 执行搜索

现在我们已经看到了一些基本的搜索参数，让我们深入研究Query DSL。让我们首先看看返回的文档字段。默认情况下，完整的JSON文档作为所有搜索的一部分返回。这被称为source \(search hits中的\_source字段\)。如果我们不希望返回整个源文档，我们可以只从源中请求返回几个字段。

这个例子展示了如何从搜索中返回两个字段account\_number和balance\(在\_source中\)：

```bash
curl -X GET "localhost:9200/bank/_search" -H 'Content-Type: application/json' -d'
{
  "query": { "match_all": {} },
  "_source": ["account_number", "balance"]
}
'
```

注意，上面的示例只是减少了\_source中的字段。它仍然返回一个名为\_source的字段，但是其中只包含account\_number和balance字段。

如果您了解SQL语句，那么上面的内容在概念上与SQL的 SELECT field from table有些类似。

现在让我们进入查询部分。在前面，我们已经了解了如何使用match\_all查询来匹配所有文档。现在让我们引入一个名为match查询的新查询，它可以被看作是基本的字段搜索查询\(即针对特定字段或字段集进行的搜索\)。

这个例子返回account\_number为20的帐户：

```bash
curl -X GET "localhost:9200/bank/_search" -H 'Content-Type: application/json' -d'
{
  "query": { "match": { "account_number": 20 } }
}
'
```

这个例子返回address中包含术语“mill”的所有帐户：

```bash
curl -X GET "localhost:9200/bank/_search" -H 'Content-Type: application/json' -d'
{
  "query": { "match": { "address": "mill" } }
}
'
```

此示例返回address中包含术语“mill”或“lane”的所有帐户：

```bash
curl -X GET "localhost:9200/bank/_search" -H 'Content-Type: application/json' -d'
{
  "query": { "match": { "address": "mill lane" } }
}
'
```

这个例子是match \(match\_phrase\)的一个变体，它返回address中包含短语“mill lane”的所有帐户：

```bash
curl -X GET "localhost:9200/bank/_search" -H 'Content-Type: application/json' -d'
{
  "query": { "match_phrase": { "address": "mill lane" } }
}
'
```

现在让我们介绍bool查询。bool查询允许我们使用布尔逻辑将较小的查询组合成较大的查询。

这个例子包含两个匹配查询，返回address中包含“mill”和“lane”的所有帐户：

```bash
curl -X GET "localhost:9200/bank/_search" -H 'Content-Type: application/json' -d'
{
  "query": {
    "bool": {
      "must": [
        { "match": { "address": "mill" } },
        { "match": { "address": "lane" } }
      ]
    }
  }
}
'
```

在上面的示例中，bool must子句指定了所有必须为true的查询，以便将文档视为匹配。

相反，这个例子包含两个匹配查询，并返回address中包含“mill”或“lane”的所有帐户：

```bash
curl -X GET "localhost:9200/bank/_search" -H 'Content-Type: application/json' -d'
{
  "query": {
    "bool": {
      "should": [
        { "match": { "address": "mill" } },
        { "match": { "address": "lane" } }
      ]
    }
  }
}
'
```

在上面的示例中，bool should子句指定了一个查询列表，其中任何一个查询必须为真，才能将文档视为匹配。

这个例子包含两个匹配查询，返回address中既不包含“mill”也不包含“lane”的所有帐户：

```bash
curl -X GET "localhost:9200/bank/_search" -H 'Content-Type: application/json' -d'
{
  "query": {
    "bool": {
      "must_not": [
        { "match": { "address": "mill" } },
        { "match": { "address": "lane" } }
      ]
    }
  }
}
'
```

在上面的例子中，bool must\_not子句指定了一个查询列表，其中没有一个查询必须为真，才能将文档视为匹配。

我们可以在bool查询中同时组合must、should和must\_not子句。此外，我们可以在这些bool子句中组合bool查询，以模拟任何复杂的多级布尔逻辑。

这个例子返回所有age是40，但state不是ID的帐户：

```bash
curl -X GET "localhost:9200/bank/_search" -H 'Content-Type: application/json' -d'
{
  "query": {
    "bool": {
      "must": [
        { "match": { "age": "40" } }
      ],
      "must_not": [
        { "match": { "state": "ID" } }
      ]
    }
  }
}
'
```

