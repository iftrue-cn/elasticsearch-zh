可以使用.zip安装包在Windows上安装Elasticsearch。elasticsearch-service.bat命令将把Elasticsearch设置为作为服务运行。
>[info] Elasticsearch过去一直使用.zip安装包安装在Windows上。现在可以使用MSI安装包，它为Windows提供了最简单的入门体验。如果您愿意，可以继续使用.zip方法。

这个包在Elastic许可下是免费使用的。它包含开源和免费的商业特性，以及对付费商业特性的访问。30天的试用，试用所有付费商业功能。有关Elastic许可级别的信息，请参阅<a href="https://www.elastic.co/cn/subscriptions" target="_blank">订阅</a>页面。

最新稳定版的Elasticsearch可以在Elasticsearch<a href="https://www.elastic.co/cn/downloads/elasticsearch" target="_blank">下载</a>页面找到。<a href="https://www.elastic.co/downloads/past-releases" target="_blank">其他版本</a>可以在以前的版本页面上找到。

### 下载安装.zip安装包
从<a href="https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.0.0-windows-x86\_64.zip" target="_blank">https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.0.0-windows-x86\_64.zip</a>下载Elasticsearch v7.0.0的.zip安装文件  

或者，您可以下载下面的包，它只包含Apache 2.0许可下可用的特性：
[https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-oss-7.0.0-windows-x86\_64.zip](https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-oss-7.0.0-windows-x86_64.zip)

下载完成后解压，得到一个名为elasticsearch-7.0.0的文件夹，我们将其称为%ES_HOME%。在终端窗口中，cd到%ES_HOME%目录，例如：
~~~bash
cd c:\elasticsearch-7.0.0
~~~

### 启用自动创建X-Pack索引
X-Pack将尝试在Elasticsearch中自动创建一些索引。默认情况下，Elasticsearch被配置为允许自动创建索引，不需要额外的步骤。但是，如果在Elasticsearch中禁用了自动索引创建，则必须配置elasticsearch.yml中的action.auto_create_index以允许X-Pack创建以下索引：
```
action.auto_create_index: .monitoring*,.watches,.triggered_watches,.watcher-history*,.ml*
```

>[warning] 如果您正在使用Logstash或Beats，那么很可能需要在操作中添加额外的action.auto_create_index设置，具体值将取决于您的本地配置。如果您不确定环境的正确值，可以考虑将值设置为*，这将允许自动创建所有索引。


### 从命令行运行Elasticsearch
Elasticsearch可以从命令行启动如下：
~~~bash
.\bin\elasticsearch.bat
~~~

默认情况下，Elasticsearch在前台运行，将其日志打印到标准输出(stdout)，并可以按Ctrl-C停止。

### 在命令行上配置Elasticsearch
Elasticsearch默认从$ES_HOME/config/elasticsearch.yml加载其配置。

任何可以在配置文件中指定的设置也可以在命令行中指定，使用 -E 语法如下：
~~~
.\bin\elasticsearch.bat -Ecluster.name=my_cluster -Enode.name=node_1
~~~
>[info] 如果值包含空格，则必须使用引号括起来，如：Epath.logs="C:\My Logs\logs"。
>[info] 通常，应该将集群范围内的任何设置(比如cluster.name)添加到elasticsearch.yml配置文件，而任何特定于节点的设置(如node.name)都可以在命令行上指定。

### 检查Elasticsearch是否正在运行
您可以通过发送一个HTTP请求到本地主机上的端口9200来测试您的Elasticsearch节点是否正在运行：
```bash
curl -X GET "localhost:9200/"
```

将会得到如下响应：
~~~
{
  "name" : "Cp8oag6",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "AT69_T_DTp-1qgIJlatQqA",
  "version" : {
    "number" : "7.0.0",
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
~~~

### 在Windows上安装Elasticsearch服务
Elasticsearch可以作为一个服务安装在后台运行，或者在启动时自动启动，不需要任何用户交互。这可以通过bin\文件夹中的elasticsearch-service.bat脚本来实现，该脚本允许安装、删除、管理或配置服务，并可能启动和停止服务，所有的这些都可以通过命令行完成。
~~~
c:\elasticsearch-7.0.0\bin>elasticsearch-service.bat

Usage: elasticsearch-service.bat install|remove|start|stop|manager [SERVICE_ID]
~~~
该脚本需要一个参数(要执行的命令)，然后是一个可选的参数，指示服务id(在安装多个Elasticsearch服务时非常有用)。
可用的命令有：
<table cellpadding="4px" border="0"><colgroup><col><col></colgroup><tbody valign="top"><tr><td valign="top">
<p>
<code class="literal">install</code>
</p>
</td><td valign="top">
<p>
将Elasticsearch安装为服务
</p>
</td></tr><tr><td valign="top">
<p>
<code class="literal">remove</code>
</p>
</td><td valign="top">
<p>
删除已安装的Elasticsearch服务(如果启动，则停止该服务)
</p>
</td></tr><tr><td valign="top">
<p>
<code class="literal">start</code>
</p>
</td><td valign="top">
<p>
启动Elasticsearch服务(如果已安装)
</p>
</td></tr><tr><td valign="top">
<p>
<code class="literal">stop</code>
</p>
</td><td valign="top">
<p>
停止Elasticsearch服务(如果启动)
</p>
</td></tr><tr><td valign="top">
<p>
<code class="literal">manager</code>
</p>
</td><td valign="top">
<p>
启动用于管理已安装服务的GUI
</p>
</td></tr></tbody></table>

服务的名称和JAVA_HOME将在安装时可用：
~~~
c:\elasticsearch-7.0.0\bin>elasticsearch-service.bat install
Installing service      :  "elasticsearch-service-x64"
Using JAVA_HOME (64-bit):  "c:\jvm\jdk1.8"
The service 'elasticsearch-service-x64' has been installed.
~~~

>[info] 虽然JRE可以用于Elasticsearch服务，但由于它使用客户机VM(而不是服务器JVM，后者为长时间运行的应用程序提供更好的性能)，因此不鼓励使用JRE，并将发出警告。

>[info] 系统环境变量JAVA_HOME应该设置为您希望服务使用的JDK安装的路径。如果升级JDK，则不需要重新安装服务，但必须将系统环境变量JAVA_HOME的值设置为新JDK安装的路径。但是，不支持跨JVM类型(例如JRE和SE)进行升级，并且确实需要重新安装服务。
> 
### 自定义设置
可以在安装之前配置Elasticsearch服务，方法是设置以下环境变量(使用命令行中的set命令，或者通过系统属性——>环境变量)。
<table cellpadding="4px" border="0"><colgroup><col><col></colgroup><tbody valign="top"><tr><td valign="top">
<p>
<code class="literal">SERVICE_ID</code>
</p>
</td><td valign="top">
<p>
  服务的唯一标识符。如果在同一台机器上安装多个实例，则非常有用。默认为 <code class="literal">elasticsearch-service-x64</code>。
</p>
</td></tr><tr><td valign="top">
<p>
<code class="literal">SERVICE_USERNAME</code>
</p>
</td><td valign="top">
<p>
 要运行的用户默认为本地系统帐户。
</p>
</td></tr><tr><td valign="top">
<p>
<code class="literal">SERVICE_PASSWORD</code>
</p>
</td><td valign="top">
<p>
  在<code class="literal">%SERVICE_USERNAME%</code>中指定的用户的密码。
</p>
</td></tr><tr><td valign="top">
<p>
<code class="literal">SERVICE_DISPLAY_NAME</code>
</p>
</td><td valign="top">
<p>
  服务的名称。默认为 <code class="literal">Elasticsearch &lt;version&gt; %SERVICE_ID%</code>。
</p>
</td></tr><tr><td valign="top">
<p>
<code class="literal">SERVICE_DESCRIPTION</code>
</p>
</td><td valign="top">
<p>
  服务的描述。默认为 <code class="literal">Elasticsearch &lt;version&gt; Windows Service - https://elastic.co</code>.
</p>
</td></tr><tr><td valign="top">
<p>
<code class="literal">JAVA_HOME</code>
</p>
</td><td valign="top">
<p>
  运行服务所需的JVM的安装目录。
</p>
</td></tr><tr><td valign="top">
<p>
<code class="literal">SERVICE_LOG_DIR</code>
</p>
</td><td valign="top">
<p>
    服务日志目录，默认为 <code class="literal">%ES_HOME%\logs</code>。这个并不是控制Elasticsearch日志的路径；Elasticsearch日志的路径是通过 <code class="literal">elasticsearch.yml</code> 配置文件中的<code class="literal">path.logs</code> 或命令行设置。
</p>
</td></tr><tr><td valign="top">
<p>
<code class="literal">ES_PATH_CONF</code>
</p>
</td><td valign="top">
<p>
    配置文件目录(需要包括<code class="literal">elasticsearch.yml</code>,
    <code class="literal">jvm.options</code>, 和<code class="literal">log4j2.properties</code> files), 默认是
    <code class="literal">%ES_HOME%\config</code>.
</p>
</td></tr><tr><td valign="top">
<p>
<code class="literal">ES_JAVA_OPTS</code>
</p>
</td><td valign="top">
<p>
    您可能希望应用的任何其他JVM系统属性。
</p>
</td></tr><tr><td valign="top">
<p>
<code class="literal">ES_START_TYPE</code>
</p>
</td><td valign="top">
<p>
    服务的启动模式。可以是自动或手动(默认)。
</p>
</td></tr><tr><td valign="top">
<p>
<code class="literal">ES_STOP_TIMEOUT</code> 
</p>
</td><td valign="top">
<p>
  procrun等待服务优雅退出的超时(以秒为单位)。默认值为<code class="literal">0</code>。
</p>
</td></tr></tbody></table>

>[info] 核心是elasticsearch-service.bat，它依赖Apache Commons守护进程项目来安装服务。在服务安装之前设置的环境变量将被复制，并将在服务生命周期中使用。这意味着除非重新安装服务，否则在安装之后对它们所做的任何更改都不会被接收。


>[info] 在Windows中，当从命令行运行Elasticsearch或第一次将Elasticsearch作为服务安装时，堆大小可以配置为任何其他Elasticsearch安装时的大小。要为已安装的服务调整堆大小，请使用服务管理器:bin\elasticsearch-service.bat管理器。


>[info] 该服务在运行时自动配置专用临时目录供Elasticsearch使用。这个私有临时目录被配置为运行安装的用户的私有临时目录的子目录。如果服务将在不同的用户下运行，则可以在执行服务安装之前将环境变量ES_TMPDIR设置为首选位置，从而配置服务应该使用的临时目录的位置。

### 使用管理器GUI
还可以在使用manager GUI (elasticsearch-service-mgr.exe)安装服务之后配置服务，该GUI提供对已安装服务的深入了解，包括其状态、启动类型、JVM、启动和停止设置等。简单地从命令行调用elasticsearch-service.bat manager就会打开管理器窗口：
![elaticsearch-service](http://source.iftrue.cn/elasticsearch/service-manager-win.png)
通过manager GUI进行的大多数更改(如JVM设置)都需要重新启动服务才能生效。


### 目录说明
默认情况下，所有文件和目录都包含在$ES_HOME（在解压缩安装时创建的目录）中。

这非常方便，因为您不需要创建任何目录就可以开始使用Elasticsearch，卸载Elasticsearch就像删除$ES_HOME目录一样简单。但是，建议更改配置目录、数据目录和日志目录的默认位置，以便以后不删除重要数据。

<table class="table table-bordered"><colgroup><col class="col_1"><col class="col_2"><col class="col_3"><col class="col_4"></colgroup><thead><tr><th align="left" valign="top"> 目录 </th><th align="left" valign="top"> 描述 </th><th align="left" valign="top"> 默认路径 </th><th align="left" valign="top"> 设置</th></tr></thead><tbody><tr><td align="left" valign="top"><p><span class="strong strong"><strong>home</strong></span></p></td><td align="left" valign="top"><p>Elasticsearch主目录或 <code class="literal">$ES_HOME</code></p></td><td align="left" valign="top"><p>解压安装包创建的目录</p></td><td align="left" valign="top"><p><code class="literal"></code></p></td></tr><tr><td align="left" valign="top"><p><span class="strong strong"><strong>bin</strong></span></p></td><td align="left" valign="top"><p>二进制脚本包括 <code class="literal">elasticsearch</code> 启动节点和插件</td><td align="left" valign="top"><p><code class="literal">$ES_HOME/bin</code></p></td><td align="left" valign="top"><p></p></td></tr><tr><td align="left" valign="top"><p><span class="strong strong"><strong>conf</strong></span></p></td><td align="left" valign="top"><p>配置文件，包括 <code class="literal">elasticsearch.yml</code></p></td><td align="left" valign="top"><p><code class="literal">$ES_HOME/config</code></p></td><td align="left" valign="top"><p><code class="literal"><a class="link" href="settings.html#config-files-location" title="Config files locationedit">ES_PATH_CONF</a></code></p></td></tr><tr><td align="left" valign="top"><p><span class="strong strong"><strong>data</strong></span></p></td><td align="left" valign="top"><p>节点上分配的每个索引/分分的数据文件的位置。可以容纳多个位置。</p></td><td align="left" valign="top"><p><code class="literal">$ES_HOME/data</code></p></td><td align="left" valign="top"><p><code class="literal">path.data</code></p></td></tr><tr><td align="left" valign="top"><p><span class="strong strong"><strong>logs</strong></span></p></td><td align="left" valign="top"><p>日志文件的位置。</p></td><td align="left" valign="top"><p><code class="literal">$ES_HOME/logs</code></p></td><td align="left" valign="top"><p><code class="literal">path.logs</code></p></td></tr><tr><td align="left" valign="top"><p><span class="strong strong"><strong>plugins</strong></span></p></td><td align="left" valign="top"><p>插件文件的位置。每个插件都包含在子目录中。</p></td><td align="left" valign="top"><p><code class="literal">$ES_HOME/plugins</code></p></td><td align="left" valign="top"><p><code class="literal"></code></p></td></tr><tr><td align="left" valign="top"><p><span class="strong strong"><strong>repo</strong></span></p></td><td align="left" valign="top"><p>共享文件系统存储库位置。可以容纳多个位置。文件系统存储库可以放在这里指定的任意目录的任意子目录中。</p></td><td align="left" valign="top"><p>未配置</p></td><td align="left" valign="top"><p><code class="literal">path.repo</code></p></td></tr></tbody></table>

### 下一步
现在您已经设置了一个测试Elasticsearch环境。在你开始认真的开发或者使用Elasticsearch进入生产之前，你必须做一些额外的设置：

* 了解如何配置Elasticsearch。
* 配置重要的Elasticsearch设置。
* 配置重要的系统设置。