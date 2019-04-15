# 集群健康状态

让我们从一个基本的健康检查开始，我们可以使用它来查看集群的运行情况。我们将使用curl来实现这一点，但是您可以使用任何允许您进行HTTP/REST调用的工具。保持Elasticsearch运行，并打开另一个命令shell窗口。

为了检查集群的健康状况，我们将使用\_cat API：

```bash
[root@localhost ~]# curl -X GET "localhost:9200/_cat/health?v"
```

响应：

```text
epoch      timestamp cluster       status node.total node.data shards pri relo init unassign pending_tasks max_task_wait_time active_shards_percent
1546931211 07:06:51  elasticsearch green           1         1      0   0    0    0        0             0                  -                100.0%
```

我们可以看到名为“elasticsearch”的集群处于绿色状态。

每当我们请求集群健康状况时，我们要么得到绿色、黄色，要么得到红色。

* 绿色——一切正常\(集群功能齐全\)
* 黄色——所有数据可用，但有些副本尚未分配\(集群功能完全\)
* 红色——有些数据由于某种原因不可用\(集群部分功能\)

注意：当集群为红色时，它将继续为可用分片的搜索请求提供服务，但是您可能需要尽快修复它，因为存在未分配的分片。

同样，从上面的响应中，我们可以看到总共有1个节点，并且有0个分片，因为其中还没有数据。注意，因为我们使用的是默认集群名称\(elasticsearch\)和由于elasticsearch使用单播网络发现默认情况下找到其他节点在同一台机器上，这是可能的，你可以不小心启动多个节点在你的电脑上，让他们加入一个集群。在此场景中，您可能会在上面的响应中看到多个节点。

我们还可以得到集群中的节点列表如下：

```bash
curl -X GET "localhost:9200/_cat/nodes?v"
```

响应：

```text
ip        heap.percent ram.percent cpu load_1m load_5m load_15m node.role master name
127.0.0.1           11          22   0    0.00    0.02     0.05 mdi       *      localhost.localdomain
```

在这里，我们可以看到一个名为“localhost.localdomain”的节点，它是当前集群中的单个节点。

