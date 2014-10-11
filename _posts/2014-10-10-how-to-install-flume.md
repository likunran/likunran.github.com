---
layout: post
title: flume 单机部署
category: flume
summary: Flume是一个分布式、可靠、和高可用的海量日志聚合的系统，支持在系统中定制各类数据发送方，用于收集数据；同时，Flume提供对数据进行简单处理，并写到各种数据接受方（可定制）的能力。
tags: [storm, mysql, mybatis]
urlId: 20141010
---

{{ page.title }}
================

{{page.summary}}

开发环境：
- jdk 1.6  
- maven
- linux 

## 下载 flume

地址 `　http://www.apache.org/dyn/closer.cgi/flume/1.5.0/apache-flume-1.5.0-bin.tar.gz`



##修改配置文件

  ｀ tar zxvf apache-flume-1.5.0-bin.tar.gz｀
    vi flume.conf
    配置如下

{% highlight text %}
a1.sources = r1
a1.sinks = k1
a1.channels = c1
 
a1.sources.r1.type = avro
a1.sources.r1.channels = c1
a1.sources.r1.bind = 0.0.0.0
a1.sources.r1.port = 4141
 
a1.sinks.k1.type = logger
 
a1.channels.c1.type = memory
a1.channels.c1.capacity = 1000
a1.channels.c1.transactionCapacity = 100
 
a1.sources.r1.channels = c1
a1.sinks.k1.channel = c1
{% endhighlight %}

修改 
cp flume-env.sh.template flume-env.sh
添加环境
java path


## 启动flume

bin/flume-ng agent -c conf -f conf/avro.conf -n a1 -Dflume.root.logger=INFO,console
{% highlight text %}
bash-4.1# bin/flume-ng agent -c conf -f conf/flume.conf -n a1 -Dflume.root.logger=INFO,console
Info: Sourcing environment configuration script /export/home/server/apache-flume-1.5.0-bin/conf/flume-env.sh
+ exec /export/home/server/jdk1.6.0_25/bin/java -Xmx20m -Dflume.root.logger=INFO,console -cp '/export/home/server/apache-flume-1.5.0-bin/conf:/export/home/server/apache-flume-1.5.0-bin/lib/*' -Djava.library.path= org.apache.flume.node.Application -f conf/flume.conf -n a1
2014-10-11 22:25:24,246 (lifecycleSupervisor-1-0) [INFO - org.apache.flume.node.PollingPropertiesFileConfigurationProvider.start(PollingPropertiesFileConfigurationProvider.java:61)] Configuration provider starting
2014-10-11 22:25:24,249 (conf-file-poller-0) [INFO - org.apache.flume.node.PollingPropertiesFileConfigurationProvider$FileWatcherRunnable.run(PollingPropertiesFileConfigurationProvider.java:133)] Reloading configuration file:conf/flume.conf
2014-10-11 22:25:24,254 (conf-file-poller-0) [INFO - org.apache.flume.conf.FlumeConfiguration$AgentConfiguration.addProperty(FlumeConfiguration.java:930)] Added sinks: k1 Agent: a1
2014-10-11 22:25:24,254 (conf-file-poller-0) [INFO - org.apache.flume.conf.FlumeConfiguration$AgentConfiguration.addProperty(FlumeConfiguration.java:1016)] Processing:k1
2014-10-11 22:25:24,255 (conf-file-poller-0) [INFO - org.apache.flume.conf.FlumeConfiguration$AgentConfiguration.addProperty(FlumeConfiguration.java:1016)] Processing:k1
2014-10-11 22:25:24,263 (conf-file-poller-0) [INFO - org.apache.flume.conf.FlumeConfiguration.validateConfiguration(FlumeConfiguration.java:140)] Post-validation flume configuration contains configuration for agents: [a1]
2014-10-11 22:25:24,263 (conf-file-poller-0) [INFO - org.apache.flume.node.AbstractConfigurationProvider.loadChannels(AbstractConfigurationProvider.java:150)] Creating channels
2014-10-11 22:25:24,274 (conf-file-poller-0) [INFO - org.apache.flume.channel.DefaultChannelFactory.create(DefaultChannelFactory.java:40)] Creating instance of channel c1 type memory
2014-10-11 22:25:24,281 (conf-file-poller-0) [INFO - org.apache.flume.node.AbstractConfigurationProvider.loadChannels(AbstractConfigurationProvider.java:205)] Created channel c1
2014-10-11 22:25:24,282 (conf-file-poller-0) [INFO - org.apache.flume.source.DefaultSourceFactory.create(DefaultSourceFactory.java:39)] Creating instance of source r1, type avro
2014-10-11 22:25:24,305 (conf-file-poller-0) [INFO - org.apache.flume.sink.DefaultSinkFactory.create(DefaultSinkFactory.java:40)] Creating instance of sink: k1, type: logger
2014-10-11 22:25:24,309 (conf-file-poller-0) [INFO - org.apache.flume.node.AbstractConfigurationProvider.getConfiguration(AbstractConfigurationProvider.java:119)] Channel c1 connected to [r1, k1]
2014-10-11 22:25:24,315 (conf-file-poller-0) [INFO - org.apache.flume.node.Application.startAllComponents(Application.java:138)] Starting new configuration:{ sourceRunners:{r1=EventDrivenSourceRunner: { source:Avro source r1: { bindAddress: 127.0.0.1, port: 4141 } }} sinkRunners:{k1=SinkRunner: { policy:org.apache.flume.sink.DefaultSinkProcessor@6719dc16 counterGroup:{ name:null counters:{} } }} channels:{c1=org.apache.flume.channel.MemoryChannel{name: c1}} }
2014-10-11 22:25:24,322 (conf-file-poller-0) [INFO - org.apache.flume.node.Application.startAllComponents(Application.java:145)] Starting Channel c1
2014-10-11 22:25:24,323 (conf-file-poller-0) [INFO - org.apache.flume.node.Application.startAllComponents(Application.java:160)] Waiting for channel: c1 to start. Sleeping for 500 ms
2014-10-11 22:25:24,352 (lifecycleSupervisor-1-0) [INFO - org.apache.flume.instrumentation.MonitoredCounterGroup.register(MonitoredCounterGroup.java:119)] Monitored counter group for type: CHANNEL, name: c1: Successfully registered new MBean.
2014-10-11 22:25:24,354 (lifecycleSupervisor-1-0) [INFO - org.apache.flume.instrumentation.MonitoredCounterGroup.start(MonitoredCounterGroup.java:95)] Component type: CHANNEL, name: c1 started
2014-10-11 22:25:24,823 (conf-file-poller-0) [INFO - org.apache.flume.node.Application.startAllComponents(Application.java:173)] Starting Sink k1
2014-10-11 22:25:24,824 (conf-file-poller-0) [INFO - org.apache.flume.node.Application.startAllComponents(Application.java:184)] Starting Source r1
2014-10-11 22:25:24,827 (lifecycleSupervisor-1-0) [INFO - org.apache.flume.source.AvroSource.start(AvroSource.java:220)] Starting Avro source r1: { bindAddress: 127.0.0.1, port: 4141 }...
2014-10-11 22:25:25,131 (lifecycleSupervisor-1-0) [INFO - org.apache.flume.instrumentation.MonitoredCounterGroup.register(MonitoredCounterGroup.java:119)] Monitored counter group for type: SOURCE, name: r1: Successfully registered new MBean.
2014-10-11 22:25:25,131 (lifecycleSupervisor-1-0) [INFO - org.apache.flume.instrumentation.MonitoredCounterGroup.start(MonitoredCounterGroup.java:95)] Component type: SOURCE, name: r1 started
2014-10-11 22:25:25,132 (lifecycleSupervisor-1-0) [INFO - org.apache.flume.source.AvroSource.start(AvroSource.java:245)] Avro source r1 started.
{% endhighlight %}

##创建文件
新建窗口 新建文件
echo "hello world" log.00

##使用avro-client发送文件
｀bin/flume-ng avro-client -c conf -H 127.0.0.1 -p 4141 -F log.00｀

查看第一个窗口
{% highlight text %}
2014-10-11 22:31:02,355 (New I/O  worker #1) [INFO - org.apache.avro.ipc.NettyServer$NettyServerAvroHandler.handleUpstream(NettyServer.java:171)] [id: 0xd0366a23, /127.0.0.1:41593 => /127.0.0.1:4141] BOUND: /127.0.0.1:4141
2014-10-11 22:31:02,356 (New I/O  worker #1) [INFO - org.apache.avro.ipc.NettyServer$NettyServerAvroHandler.handleUpstream(NettyServer.java:171)] [id: 0xd0366a23, /127.0.0.1:41593 => /127.0.0.1:4141] CONNECTED: /127.0.0.1:41593
2014-10-11 22:31:02,750 (New I/O  worker #1) [INFO - org.apache.avro.ipc.NettyServer$NettyServerAvroHandler.handleUpstream(NettyServer.java:171)] [id: 0xd0366a23, /127.0.0.1:41593 :> /127.0.0.1:4141] DISCONNECTED
2014-10-11 22:31:02,750 (New I/O  worker #1) [INFO - org.apache.avro.ipc.NettyServer$NettyServerAvroHandler.handleUpstream(NettyServer.java:171)] [id: 0xd0366a23, /127.0.0.1:41593 :> /127.0.0.1:4141] UNBOUND
2014-10-11 22:31:02,750 (New I/O  worker #1) [INFO - org.apache.avro.ipc.NettyServer$NettyServerAvroHandler.handleUpstream(NettyServer.java:171)] [id: 0xd0366a23, /127.0.0.1:41593 :> /127.0.0.1:4141] CLOSED
2014-10-11 22:31:02,750 (New I/O  worker #1) [INFO - org.apache.avro.ipc.NettyServer$NettyServerAvroHandler.channelClosed(NettyServer.java:209)] Connection to /127.0.0.1:41593 disconnected.
2014-10-11 22:31:06,891 (SinkRunner-PollingRunner-DefaultSinkProcessor) [INFO - org.apache.flume.sink.LoggerSink.process(LoggerSink.java:70)] Event: { headers:{} body: 68 65 6C 6C 6F 20 77 6F 72 6C 64                hello world }
{% endhighlight %}







#总结
收集成功
