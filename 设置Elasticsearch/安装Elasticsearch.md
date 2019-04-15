### 托管Elasticsearch
您可以在自己的硬件上运行Elasticsearch，或者使用各种云计算平台提供的Elasticsearch服务。

### 自己安装Elasticsearch
Elasticsearch提供以下包格式：
<table class="table  table-bordered"><colgroup><col><col></colgroup><tbody><tr><td>
<p>
Linux and MacOS <code class="literal">tar.gz</code> archives
</p>
</td><td valign="top">
<p>
可以在任何Linux发行版和MacOS上安装tar.gz包。
</p>
</td></tr><tr><td valign="top">
<p>
Windows <code class="literal">.zip</code> archive
</p>
</td><td valign="top">
<p>
zip文件适合安装在Windows上。
</p>
</td></tr><tr><td valign="top">
<p>
<code class="literal">deb</code>
</p>
</td><td valign="top">
<p>
deb包适用于Debian、Ubuntu和其他基于Debian的系统。Debian包可以从Elasticsearch网站或Debian存储库下载。
</p>
</td></tr><tr><td valign="top">
<p>
<code class="literal">rpm</code>
</p>
</td><td valign="top">
<p>
rpm包适合安装在Red Hat、Centos、SLES、OpenSuSE等基于rpm的系统上。RPM可以从Elasticsearch网站或RPM存储库下载。
</p>
</td></tr><tr><td valign="top">
<p>
<code class="literal">msi</code>
</p>
</td><td valign="top">
<p>
<span class="beta">
      [<span class="beta_title">beta</span>]
      <span class="detail">msi包适合安装在至少安装了. net 4.5框架的Windows 64位系统上，是在Windows上开始使用Elasticsearch的最简单选择。MSI可从Elasticsearch网站下载。</span></span>
</p>
</td></tr><tr><td valign="top">
<p>
<code class="literal">docker</code>
</p>
</td><td valign="top">
<p>
镜像可以作为Docker容器运行Elasticsearch。它们可以从Elastic Docker Registry下载。
</p>
</td></tr></tbody></table>

### 配置管理工具
提供以下配置管理工具，以帮助大型部署：
<table  class="table  table-bordered"><colgroup><col><col></colgroup><tbody valign="top"><tr><td valign="top">
<p>
Puppet
</p>
</td><td valign="top">
<p>
puppet-elasticsearch
</p>
</td></tr><tr><td valign="top">
<p>
Chef
</p>
</td><td valign="top">
<p>
cookbook-elasticsearch
</p>
</td></tr><tr><td valign="top">
<p>
Ansible
</p>
</td><td valign="top">
<p>
ansible-elasticsearch
</p>
</td></tr></tbody></table>