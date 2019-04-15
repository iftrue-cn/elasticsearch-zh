### 在Linux或MacOS上安装Elasticsearch
Elasticsearch在Linux和MacOS系统使用.tar.gz包安装。

这个包在Elastic许可下是免费使用的。它包含开源和免费的商业特性，以及对付费商业特性的访问。30天的试用，试用所有付费商业功能。有关Elastic许可级别的信息，请参阅<a href="https://www.elastic.co/cn/subscriptions" target="_blank">订阅</a>页面。

最新稳定版的Elasticsearch可以在Elasticsearch<a href="https://www.elastic.co/cn/downloads/elasticsearch" target="_blank">下载</a>页面找到。<a href="https://www.elastic.co/downloads/past-releases" target="_blank">其他版本</a>可以在以前的版本页面上找到。

### 在Linux中下载、安装
Elasticsearch v7.0.0的Linux安装包可以下载并安装如下：
```bash
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.0.0-linux-x86_64.tar.gz
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.0.0-linux-x86_64.tar.gz.sha512
shasum -a 512 -c elasticsearch-7.0.0-linux-x86_64.tar.gz.sha512 
tar -xzf elasticsearch-7.0.0-linux-x86_64.tar.gz
cd elasticsearch-7.0.0/ 
```

或者，您可以下载以下包，其中只包含Apache 2.0授权代码：

<a href="https://artifacts.elastic.co/downloads/elasticsearch/elasticsears-oss-7.0.0 -linux-x86_64.tar.gz" target="_blank">https://artifacts.elastic.co/downloads/elasticsearch/elasticsears-oss-7.0.0 -linux-x86_64.tar.gz</a>

### 在MacOS中下载、安装
Elasticsearch v7.0.0的MacOS安装包可以下载并安装如下：
```bash
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.0.0-darwin-x86_64.tar.gz
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.0.0-darwin-x86_64.tar.gz.sha512
shasum -a 512 -c elasticsearch-7.0.0-darwin-x86_64.tar.gz.sha512 
tar -xzf elasticsearch-7.0.0-darwin-x86_64.tar.gz
cd elasticsearch-7.0.0/ 
```

或者，您可以下载以下包，其中只包含Apache 2.0授权代码：
<a href="https://artifacts.elastic.co/downloads/elasticsearch/elasticsears-oss-7.0.0 - darwinx86_64 .tar.gz" target="_blank">https://artifacts.elastic.co/downloads/elasticsearch/elasticsears-oss-7.0.0 - darwinx86_64 .tar.gz</a>

安装后将elasticsearch根目录配置为环境变量：$ES_HOME

### 启用自动创建X-Pack索引
X-Pack将尝试在Elasticsearch中自动创建一些索引。默认情况下，Elasticsearch被配置为允许自动创建索引，不需要额外的步骤。但是，如果在Elasticsearch中禁用了自动索引创建，则必须配置elasticsearch.yml中的action.auto_create_index以允许X-Pack创建以下索引：
```
action.auto_create_index: .monitoring*,.watches,.triggered_watches,.watcher-history*,.ml*
```

>[warning] 如果您正在使用Logstash或Beats，那么很可能需要在操作中添加额外的action.auto_create_index设置，具体值将取决于您的本地配置。如果您不确定环境的正确值，可以考虑将值设置为*，这将允许自动创建所有索引。

### 从命令行运行Elasticsearch
Elasticsearch可以从命令行启动如下：
```bash
./bin/elasticsearch
```

默认情况下，Elasticsearch在前台运行，将其日志打印到标准输出(stdout)，并可以按Ctrl-C停止。

### 检查Elasticsearch是否正在运行
您可以通过发送一个HTTP请求到本地主机上的端口9200来测试您的Elasticsearch节点是否正在运行：
```bash
curl -X GET "localhost:9200/"
```

将会得到如下响应：
```
{
  "name" : "Cp8oag6",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "AT69_T_DTp-1qgIJlatQqA",
  "version" : {
    "build_flavor" : "default",
    "build_type" : "tar",
    "build_hash" : "f27399d",
    "build_date" : "2016-03-30T09:51:41.449Z",
    "build_snapshot" : false,
    "lucene_version" : "8.0.0",
    "minimum_wire_compatibility_version" : "1.2.3",
    "minimum_index_compatibility_version" : "1.2.3"
  },
  "tagline" : "You Know, for Search"
}
```

可以使用命令行上的-q或——quiet选项禁用将日志打印到stdout。

### 作为守护进程运行
要作为守护进程运行Elasticsearch，请在命令行中指定-d，并使用-p选项将进程ID记录到文件中：
```bash
./bin/elasticsearch -d -p pid
```

可以在$ES_HOME/logs/目录中找到日志消息。

若要关闭Elasticsearch，请杀死pid文件中记录的进程ID：
```bash
pkill -F pid
```

注：RPM和Debian包中提供的启动脚本负责启动和停止Elasticsearch过程。

### 在命令行上配置Elasticsearch
Elasticsearch默认从$ES_HOME/config/elasticsearch.yml加载其配置。

任何可以在配置文件中指定的设置也可以在命令行中指定，使用 -E 语法如下：
```bash
./bin/elasticsearch -d -Ecluster.name=my_cluster -Enode.name=node_1
```

注：通常，应该将集群范围内的任何设置(比如cluster.name)添加到elasticsearch.yml配置文件，而任何特定于节点的设置(如node.name)都可以在命令行上指定。

### 目录说明
默认情况下，所有文件和目录都包含在$ES_HOME（在解压缩安装时创建的目录）中。

这非常方便，因为您不需要创建任何目录就可以开始使用Elasticsearch，卸载Elasticsearch就像删除$ES_HOME目录一样简单。但是，建议更改配置目录、数据目录和日志目录的默认位置，以便以后不删除重要数据。

<table class="table table-bordered"><colgroup><col class="col_1"><col class="col_2"><col class="col_3"><col class="col_4"></colgroup><thead><tr><th align="left" valign="top"> 目录 </th><th align="left" valign="top"> 描述 </th><th align="left" valign="top"> 默认路径 </th><th align="left" valign="top"> 设置</th></tr></thead><tbody><tr><td align="left" valign="top"><p><span class="strong strong"><strong>home</strong></span></p></td><td align="left" valign="top"><p>Elasticsearch主目录或 <code class="literal">$ES_HOME</code></p></td><td align="left" valign="top"><p>解压安装包创建的目录</p></td><td align="left" valign="top"><p><code class="literal"></code></p></td></tr><tr><td align="left" valign="top"><p><span class="strong strong"><strong>bin</strong></span></p></td><td align="left" valign="top"><p>二进制脚本包括 <code class="literal">elasticsearch</code> 启动节点和插件</td><td align="left" valign="top"><p><code class="literal">$ES_HOME/bin</code></p></td><td align="left" valign="top"><p></p></td></tr><tr><td align="left" valign="top"><p><span class="strong strong"><strong>conf</strong></span></p></td><td align="left" valign="top"><p>配置文件，包括 <code class="literal">elasticsearch.yml</code></p></td><td align="left" valign="top"><p><code class="literal">$ES_HOME/config</code></p></td><td align="left" valign="top"><p><code class="literal"><a class="link" href="settings.html#config-files-location" title="Config files locationedit">ES_PATH_CONF</a></code></p></td></tr><tr><td align="left" valign="top"><p><span class="strong strong"><strong>data</strong></span></p></td><td align="left" valign="top"><p>节点上分配的每个索引/分分的数据文件的位置。可以容纳多个位置。</p></td><td align="left" valign="top"><p><code class="literal">$ES_HOME/data</code></p></td><td align="left" valign="top"><p><code class="literal">path.data</code></p></td></tr><tr><td align="left" valign="top"><p><span class="strong strong"><strong>logs</strong></span></p></td><td align="left" valign="top"><p>日志文件的位置。</p></td><td align="left" valign="top"><p><code class="literal">$ES_HOME/logs</code></p></td><td align="left" valign="top"><p><code class="literal">path.logs</code></p></td></tr><tr><td align="left" valign="top"><p><span class="strong strong"><strong>plugins</strong></span></p></td><td align="left" valign="top"><p>插件文件的位置。每个插件都包含在子目录中。</p></td><td align="left" valign="top"><p><code class="literal">$ES_HOME/plugins</code></p></td><td align="left" valign="top"><p><code class="literal"></code></p></td></tr><tr><td align="left" valign="top"><p><span class="strong strong"><strong>repo</strong></span></p></td><td align="left" valign="top"><p>共享文件系统存储库位置。可以容纳多个位置。文件系统存储库可以放在这里指定的任意目录的任意子目录中。</p></td><td align="left" valign="top"><p>未配置</p></td><td align="left" valign="top"><p><code class="literal">path.repo</code></p></td></tr><tr><td align="left" valign="top"><p><span class="strong strong"><strong>script</strong></span></p></td><td align="left" valign="top"><p>脚本文件的位置。</p></td><td align="left" valign="top"><p><code class="literal">$ES_HOME/scripts</code></p></td><td align="left" valign="top"><p><code class="literal">path.scripts</code></p></td></tr></tbody></table>

### 下一步
现在您已经设置了一个测试Elasticsearch环境。在你开始认真的开发或者使用Elasticsearch进入生产之前，你必须做一些额外的设置：

* 了解如何配置Elasticsearch。
* 配置重要的Elasticsearch设置。
* 配置重要的系统设置。