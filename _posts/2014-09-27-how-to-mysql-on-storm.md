---
layout: post
title: storm 结合 mybatis 数据 入库
category: storm
summary: storm strom-starter-master  项目WordCountTopology 进行操作数据扩展。使用 druid作为数据源 用spring+mybatis 计算结果写到msql中。
tags: [storm, jekyll, rdiscount]
urlId: 20140927
---

{{ page.title }}
================

{{page.summary}}

机子上的基本环境：

- mysql  
- idea

##mysql
  1.本地已经安装 mysql 启动mysql
    

  {% highlight text %}
  bash-4.1#   /etc/init.d/mysqld  start
  正在启动 mysqld： [确定]
 {% endhighlight %}

 执行：` mysql -uroot -p123456`，提示：

{% highlight text %}
Copyright (c) 2000, 2013, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> 

{% endhighlight %}

2.建立数据库和数据表 完成后结果如下
{% highlight text %}
mysql> use uyicu
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> desc system_log
    -> ;
+-------------+---------------+------+-----+---------+----------------+
| Field       | Type          | Null | Key | Default | Extra          |
+-------------+---------------+------+-----+---------+----------------+
| id          | int(4)        | NO   | PRI | NULL    | auto_increment |
| log_name    | varchar(1000) | YES  |     | NULL    |                |
| log_value   | varchar(1000) | YES  |     | NULL    |                |
| create_time | datetime      | YES  |     | NULL    |                |
| modify_time | datetime      | YES  |     | NULL    |                |
| yn          | int(11)       | YES  |     | NULL    |                |
+-------------+---------------+------+-----+---------+----------------+
6 rows in set (0.00 sec)

mysql> 
{% endhighlight %}

##strom-starter 添加 pom依赖


{% highlight text %}
<dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-core</artifactId>
                <version>${spring.version}</version>
            </dependency>
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-beans</artifactId>
                <version>${spring.version}</version>
            </dependency>
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-context</artifactId>
                <version>${spring.version}</version>
            </dependency>
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-context-support</artifactId>
                <version>${spring.version}</version>
            </dependency>

            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-aop</artifactId>
                <version>${spring.version}</version>
            </dependency>
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-aspects</artifactId>
                <version>${spring.version}</version>
            </dependency>
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-tx</artifactId>
                <version>${spring.version}</version>
            </dependency>
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-jdbc</artifactId>
                <version>${spring.version}</version>
            </dependency>
              <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.26</version>
        </dependency>

        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <version>1.2.0</version>
        </dependency>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.2.2</version>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>0.2.25</version>
        </dependency>
{% endhighlight %}


##添加spring+mybatis配置文件
   mybatis 配置大家开参考 spring mybatis配置篇
   添加 spring-config.xml
       spring-config-dao.xml

##添加SpringUtils
   springUtils用于加载spring 配置
  ｀

    public static void build() {

        if (null == factory) {
            factory = new ClassPathXmlApplicationContext("spring-config.xml");
        }

    }
｀

##storm RandomSentenceSpout 代码修改
{% highlight text %}

private ULogMapper uLogMapper;


    /**
     * open 方法用于初始化
     * @param conf
     * @param context
     * @param collector
     */
  @Override
  public void open(Map conf, TopologyContext context, SpoutOutputCollector collector) {
    _collector = collector;

      uLogMapper= SpringUtils.uLogMapperInstance();
      _rand = new Random();
  }

    /**
     * 在SpoutTracker类中被调用，每调用一次就可以向storm集群中发射一条数据（一个Tuple元组），该方法会被不停的调用
     */
  @Override
  public void nextTuple() {



      ULog uLog=new ULog();
      uLog.setCreateTime(new Date());
      uLog.setLogName("nameSpout");
      uLog.setLogValue("value");
      uLogMapper.insert(uLog) ;

    Utils.sleep(100);
    String[] sentences = new String[]{ "the cow jumped over the moon", "an apple a day keeps the doctor away",
        "four score and seven years ago", "snow white and the seven dwarfs", "i am at two with nature" };
    String sentence = sentences[_rand.nextInt(sentences.length)];
    _collector.emit(new Values(sentence));
  }
  {% endhighlight %}



#总结
在本地模式运行成功 
