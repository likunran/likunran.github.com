---
layout: post
title: mongodb 安装
category: mongodb
summary: 项目中日志量增大。准备把日志存储起来 想把日志存到hbase但是rowkey设计比较麻烦。而且日之后查询到纬度也比较多。今天尝试把日志先写到redis 然后用队列在同步到mongodb.
 
tags: [mongodb, jekyll, rdiscount]
urlId: 20140928
---

{{ page.title }}
================

{{page.summary}}

机子上的基本环境：

- centos  
- mongodb

##下载
  下载mongodb-linux-x86_64-2.4.9.tgz
    
  修改配置文件
  
  {% highlight text %}
  
tar zxvf mongodb-linux-x86_64-2.4.9.tgz
mv mongodb-linux-x86_64-2.4.9 mongodb
cd mongodb
mkdir db
mkdir logs
cd bin


 {% endhighlight %}
 
##配置mongodb
新建配置文件
  {% highlight text %}
  vi mongodb.conf
添加配置文件
dbpath = /export/data/mongo/db            
logpath = /export/data/mongo/log
logappend = true                  
 
port = 27017                     
fork = true                        
 {% endhighlight %}


## mongodb

  ./mongod --config mongodb.conf  
  bin/mongo
  进入shell命令后
  
  ` 
use likunran //切换数据库不存在不存在创建
db.addUser(“root”,"123456")

db.auth("root","123456”)  
 `
 
重启 mongodb

使用账号和密码登录mongodb
mongo likunran -uroot -p123456

show dbs
use likunran //切换数据库不存在不存在创建
show collections
db.likunran.insert({'name':'likunran'});
show collections
db.likunran.find()

#总结
在本地模式运行成功 
