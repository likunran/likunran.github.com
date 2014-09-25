---
layout: post
title: Centos 下 storm 单机部署
category: storm
summary: 从去年做广告效果统计接触storm开始。开始了解了storm 也有一段时间了。这段时间想在自己机器上部署个storm 但是发现网上没有太好点教程就决定自己写一篇单机部署。
tags: [storm, jekyll, rdiscount]
urlId: 20140918
---

{{ page.title }}
================

{{page.summary}}

机子上的基本环境：

- parallels 虚拟机
- centos  
- python 2.6

##jdk 替换
  1.首先卸载centos自带openjdk.
  2.官网下载jdk .
  3.配置环境变量 `vi /etc/profile`

  {% highlight text %}
You are missing a library required for Markdown. Please run:ls
export JAVA_HOME=/export/home/server/jdk1.6.0_25
export CLASSPATH=$JAVA_HOME/jre/lib:$JAVA_HOME/lib
export PATH=$PATH:$JAVA_HOME/bin
{% endhighlight %}

运行 java：`java -version`，提示：

{% highlight text %}
You are missing a library required for Markdown. Please run:
java version "1.6.0_65"
Java(TM) SE Runtime Environment (build 1.6.0_65-b14-462-11M4609)
Java HotSpot(TM) 64-Bit Server VM (build 20.65-b04-462, mixed mode)
{% endhighlight %}

##python 安装
运行 ｀python -v｀ 命令查看python 版本

{% highlight text %}
# installing zipimport hook
import zipimport # builtin
# installed zipimport hook
# /usr/lib64/python2.6/site.pyc matches /usr/lib64/python2.6/site.py
{% endhighlight %}

python 2.6 版本已经存在无需安装


##ZooKeeper 安装
   1.官网下载zookeeper

   修改配置文件如下

{% highlight text %}
tickTime=2000
dataDir=/home/hadoop/storage/zookeeper
clientPort=2181
initLimit=5
syncLimit=2
{% endhighlight %}

具体的配置项的解释大家可以自己去查

##ZeroMQ 安装

从 http://download.zeromq.org/zeromq-2.1.7.tar.gz 下载 
{% highlight text %}
tar zxf zeromq-2.1.7.tar.gz
cd zeromq-2.1.7
./configure
make
make install
sudo ldconfig
{% endhighlight %}

##jzmq 安装
从http://github.com/nathanmarz/jzmq页面下载jzmq-master.zip

{% highlight text %}
unzip -zxvf jzmq-master.zip
cd jzmq
./autogen.sh
./configure
make
make install
{% endhighlight %}

##Strom 安装

 我下载的是 0.9.0.1 这个版本可以选择是 jzmq或netty
 修改配置文件如下

{% highlight text %}
storm.zookeeper.servers:
     - "localhost"

storm.zookeeper.port: 2181
storm.local.dir: /export/data/storm/tmp
nimbus.host: "localhost"
supervisor.slots.ports:
    - 6700
    - 6701
    - 6702
    - 6703
{% endhighlight %}

由于我现在是单机部署 所以配置loccalhost  zookeeper.port 配置 刚才　zk 端口 2181

启动 strom 
{% highlight text %}
storm nimbus&
storm supervisor&
storm ui&
{% endhighlight %}

查看日志
｀tail -f ui.log｀
{% highlight text %}
2014-09-18 22:09:40 o.m.log [INFO] Logging to Logger[org.mortbay.log] via org.mortbay.log.Slf4jLog
2014-09-18 22:09:40 o.m.log [INFO] Started SocketConnector@0.0.0.0:8080
{% endhighlight %}

访问strom ui

http://10.37.129.3:8080/
图如下
<img src="/css/images/sui.png" alt="Alt text" height="300">

#总结
在安装过程中 zeromq 刚开始报错很多。根据提示一步步装其他软件就可以搞定。在外部访问虚拟机strom ui时 记得把端口打开
