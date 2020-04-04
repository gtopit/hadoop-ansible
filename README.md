**本文档适用centos7下采用ansible自动化部署hadoop(单master)、hbase集群**

---
**开始之前**  
&nbsp;&nbsp;&nbsp;&nbsp;安装包的目录和安装目录需要分开  
&nbsp;&nbsp;&nbsp;&nbsp;如需要配置免密登录，首先确保被控主机存在ssh所需的公钥和私钥，并且所有主机之间已经开启允许ssh远程登录。  
&nbsp;&nbsp;&nbsp;&nbsp;如果不满足以上条件，则可以参考下面操作：  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1、开启允许ssh远程登录（所有主机）  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;打开/etc/ssh/sshd_config，找到*PasswordAuthentication*，将其设置为*yes*，执行*systemctl restart sshd*命令重启ssh服务。  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2、配置公钥和私钥（被控主机）  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;使用*ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa*命令生成公钥和私钥  
&nbsp;&nbsp;&nbsp;&nbsp;如需要安装jdk，请提供jdk压缩包  
&nbsp;&nbsp;&nbsp;&nbsp;首先更新下yum源，*yum install -y epel-release*，然后安装ansible，*yum install -y ansible*   
&nbsp;&nbsp;&nbsp;&nbsp;打开/etc/ansible/ansible.cfg，将*host_key_checking = False*放开（去掉注释）。  

---
**使用示例1**  
&nbsp;&nbsp;&nbsp;&nbsp;自动化一键部署jdk、zookeeper、Hadoop、hbase  
&nbsp;&nbsp;&nbsp;&nbsp;1、涉及到的服务器及软件安装清单如下：
主机|主机名|部署软件
:---:|:---:|:---:
192.168.233.4|zk01、node1|zookeeper、Hadoop(namenode、secondarynamenode)、hbase(master)
192.168.233.5|zk02、node2|zookeeper、Hadoop(datanode)、hbase(backup master、regionserver)
192.168.233.6|zk03、node3|zookeeper、Hadoop(datanode)、hbase(regionserver)  
&nbsp;&nbsp;&nbsp;&nbsp;如果想要自定义软件安装的服务器，请修改hosts文件；如果想要修改其他配置，请修改group_vars/all.yml文件  
