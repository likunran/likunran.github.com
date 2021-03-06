---
layout: post
title: kafka单机部署
category: kafka
summary:  kafka是一种高吞吐量的分布式发布订阅消息系统,kafka是linkedin用于日志处理的分布式消息队列，linkedin的日志数据容量大，但对可靠性要求不高，其日志数据主要包括用户行为（登录、浏览、点击、分享、喜欢）以及系统运行日志（CPU、内存、磁盘、网络、系统及进程状态） 
tags: [kafka]
urlId: 20140918
---

{{ page.title }}
================

{{page.summary}}

官网：

- <a href='http://kafka.apache.org/'>http://kafka.apache.org/</a>

##环境配置
jdk zookeeper 配置可参照 前面文章配置。
<a href='/2014/09/18/how-to-step-storm-on-centos.html' >单机部署storm</a>

##kafka 安装



wget http://apache.fayea.com/apache-mirror/kafka/0.8.1.1/kafka_2.9.2-0.8.1.1.tgz
tar -zxf kafka_2.9.2-0.8.1.1.tgz
cd kafka_2.9.2-0.8.1.1/config/

修改配置文件
{% highlight text %}
broker.id=0
port=9092
host.name=10.37.129.3
num.network.threads=2
num.io.threads=8
socket.send.buffer.bytes=1048576
socket.request.max.bytes=104857600
log.dirs=/tmp/kafka-logs
num.partitions=2
log.flush.interval.messages=10000
log.flush.interval.ms=1000
log.retention.hours=168
log.retention.bytes=1073741824
log.segment.bytes=536870912
log.retention.check.interval.ms=60000
log.cleaner.enable=false
zookeeper.connect=10.37.129.3:2181 
zookeeper.connection.timeout.ms=1000000
{% endhighlight %}

启动

./kafka-server-start.sh /export/home/server/kafka_2.8.0-0.8.1.1/config/server.properties 


{% highlight text %}
[2014-10-28 23:57:56,694] INFO [Socket Server on Broker 0], Started (kafka.network.SocketServer)
[2014-10-28 23:57:56,766] INFO Will not load MX4J, mx4j-tools.jar is not in the classpath (kafka.utils.Mx4jLoader$)
[2014-10-28 23:57:56,788] INFO 0 successfully elected as leader (kafka.server.ZookeeperLeaderElector)
[2014-10-28 23:57:56,918] INFO New leader is 0 (kafka.server.ZookeeperLeaderElector$LeaderChangeListener)
[2014-10-28 23:57:56,920] INFO Registered broker 0 at path /brokers/ids/0 with address 10.37.129.3:9092. (kafka.utils.ZkUtils$)
[2014-10-28 23:57:56,935] INFO [Kafka Server 0], started (kafka.server.KafkaServer)

{% endhighlight %}

##kafka 常用命令


新建一个TOPIC

./kafka-topics.sh --create --topic kafkatopic --replication-factor 1 --partitions 1 --zookeeper localhost:2181

发送消息至KAFKA

./kafka-console-producer.sh --broker-list localhost:9092 --sync --topic kafkatopic

另开一个终端，显示消息的消费

./kafka-console-consumer.sh --zookeeper localhost:2181 --topic kafkatopic --from-beginning

 查看kafka topics list
 bin/kafka-topics.sh --list --zookeeper localhost:2181
删除
 bin/kafka-run-class.sh kafka.admin.DeleteTopicCommand -topic topic_001 --zookeeper localhost:2181


#总结
网上到不符合自己流程。简单记录吧。
