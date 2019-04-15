如何设置Elasticsearch并使其运行的信息，包括:

* 下载
* 安装
* 开始
* 配置

### 支持的平台
这里提供了官方支持的操作系统和jvm清单：<a href="https://www.elastic.co/cn/support/matrix" target="_blank">Support Matrix</a>。Elasticsearch在列出的平台上进行了测试，但也有可能在其他平台上运行。

### Java（JVM）版本
Elasticsearch使用Java构建，并在每个发行版中包含了OpenJDK。绑定的JVM存在于Elasticsearch主目录的jdk目录中。

要使用自己的Java版本，需要设置JAVA_HOME环境变量。当使用自己的版本时，可能会删除绑定的JVM目录。如果不使用绑定的JVM，建议安装Java version 1.8.0_131或Java 8发行版系列中的更高版本。建议使用受支持的Java LTS版本。如果使用known-bad版本，Elasticsearch将拒绝启动。