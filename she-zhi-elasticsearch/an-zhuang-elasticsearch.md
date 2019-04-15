# 安装Elasticsearch

## 托管Elasticsearch

您可以在自己的硬件上运行Elasticsearch，或者使用各种云计算平台提供的Elasticsearch服务。

## 自己安装Elasticsearch

Elasticsearch提供以下包格式：

|  Linux and MacOS `tar.gz` archives |  可以在任何Linux发行版和MacOS上安装tar.gz包。 |
| :--- | :--- |
|  Windows `.zip` archive |  zip文件适合安装在Windows上。 |
|  `deb` |  deb包适用于Debian、Ubuntu和其他基于Debian的系统。Debian包可以从Elasticsearch网站或Debian存储库下载。 |
|  `rpm` |  rpm包适合安装在Red Hat、Centos、SLES、OpenSuSE等基于rpm的系统上。RPM可以从Elasticsearch网站或RPM存储库下载。 |
|  `msi` |  \[beta\] msi包适合安装在至少安装了. net 4.5框架的Windows 64位系统上，是在Windows上开始使用Elasticsearch的最简单选择。MSI可从Elasticsearch网站下载。 |
|  `docker` |  镜像可以作为Docker容器运行Elasticsearch。它们可以从Elastic Docker Registry下载。 |

## 配置管理工具

提供以下配置管理工具，以帮助大型部署：

|  Puppet |  puppet-elasticsearch |
| :--- | :--- |
|  Chef |  cookbook-elasticsearch |
|  Ansible |  ansible-elasticsearch |

