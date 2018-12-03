## Welcome to Hadoop学习笔记

# 一、 Hadoop安装
### Hadoop集群可以部署在物理服务器上，也可以部署在虚拟机里，也可以部署在容器里，这笔使用容器进行Hadoop安装学习
注意：下载已经安装了Hadoop的容器镜像，就不需要再安装部署Hadoop软件
## 1.1 安装JDK
### Hadoop是使用java开发的，java是必须安装的软件之一。在CentOS环境中可以通过yum进行安装或者去官网下载最新版本的JDK。容器在编译时已经安装了JDK，不需要再安装JDK了。如果其他容器，还是需要安装JDK。源码安装从官方网站下载Linux x64的jdk-8u131-linux-x64.tar.gz或更高版本。
> 下载地址：http://www.oracle.com/technetwork/java/javase/downloads/index.html
### 1.1.1解压安装:
```
tar zxvf jdk-8u131-linux-x64.tar.gz -C /usr/local/
ls /usr/local/jdk1.8.0_131/
```
### 1.1.2设置环境变量:
```
vim /etc/profile			添加以下内容
#JAVA
export JAVA_HOME=/usr/local/jdk1.8.0_131
export JRE_HOME=/usr/local/jdk1.8.0_131/jre 
export CLASSPATH=$JAVA_HOME/lib:$JRE_HOME/lib:$CLASSPATH:.
export PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH
source /etc/profile
java -version
```
### source /etc/profile使环境变量生效，查看java版本，如能看到版本号，说明JAVA环境配置成功。
## 1.2 安装hadoop
### 可以从官方网站下载编译好的hadoop(http://hadoop.apache.org/releases.html#Download),但是官方提供的已经编译好的hadoop，在CentOS 64位机器上使用是有问题的，我们需要自己编译的hadoop。
注意：容器在编译时已经安装了hadoop，不需要再安装hadoop了。如果其他容器，还是需要安装hadoop。
### 1.2.1 从交大服务器上下载编译好的hadoop
```
wget http://202.120.32.244/bigdata/software/hadoop-2.7.3-hongdy.tar.gz
```
### 1.2.2 解压安装
```
tar zxvf hadoop-2.7.3-hongdy.tar.gz -C /usr/local/
```
### 1.2.3 设置环境变量
```
vim /etc/profile				最后添加以下内容
# hadoop
export HADOOP_HOME=/usr/local/hadoop-2.7.3
export HADOOP_PREFIX=$HADOOP_HOME
export HADOOP_COMMON_HOME=$HADOOP_PREFIX
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_PREFIX/lib/native
export HADOOP_CONF_DIR=$HADOOP_PREFIX/etc/hadoop
export HADOOP_HDFS_HOME=$HADOOP_PREFIX
export HADOOP_MAPRED_HOME=$HADOOP_PREFIX
export HADOOP_YARN_HOME=$HADOOP_PREFIX
export LD_LIBRARY_PATH=$HADOOP_PREFIX/lib/native:$LD_LIBRARY_PATH
export PATH=$HADOOP_HOME/bin:$PATH
```
### 1.2.4 查看版本
```
source /etc/profile
/usr/local/hadoop-2.7.3/bin/hadoop version
```
### source /etc/profile使环境变量生效；查看hadoop版本，如能看到版本号，说明hadoop环境配置成功。
## 1.3 Hadoop容器环境
### 1.3.1 安装容器-设置容器安装仓库源
```
vim /etc/yum.repos.d/sjtu-docker.repo 			内容如下所示：
[sjtu-docker-repo]
name=Docker Repository
#baseurl=https://yum.dockerproject.org/repo/main/centos/7/
baseurl=http://202.120.32.244/repos/docker/centos/7/
enabled=1
gpgcheck=0
gpgkey=https://yum.dockerproject.org/gpg
```
### 1.3.2 安装docker-engine
```
yum -y install docker-engine
```
### 1.3.3 启动docker
```
systemctl restart docker.service
systemctl enable docker.service
```
### 1.3.4 下载导入容器镜像
```
mkdir -p /root/docker/images
cd /root/docker/images
wget http://202.120.32.244/docker/images/docker-centos6.10-hadoop-spark.tar
```
### 1.3.5 导入容器镜像
```
docker load < docker-centos6.10-dekstop-hadoop-spark.tar
```
## 1.4 运行Hadoop容器
### 1.4.1 删除已经运行的所有容器
```
docker kill $(docker ps -a -q) 
docker rm $(docker ps -a -q) 
```
### 1.4.2 运行容器
#### 伪分布式集群（hadoop20）
```
docker run -d --name='hadoop20' --hostname='hadoop20'  docker-centos6.10-hadoop-spark
```
#### 完全分布式集群（hadoop21、hadoop22、hadoop23、hadoop24）
```
docker run -d --name='hadoop21' --hostname='hadoop21'  docker-centos6.10-hadoop-spark
docker run -d --name='hadoop22' --hostname='hadoop22'  docker-centos6.10-hadoop-spark
docker run -d --name='hadoop23' --hostname='hadoop23'  docker-centos6.10-hadoop-spark
docker run -d --name='hadoop24' --hostname='hadoop24'  docker-centos6.10-hadoop-spark
```



