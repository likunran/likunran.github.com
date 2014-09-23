---
layout: post
title: storm提交程序
category: storm
summary: storm 已经部署完成准备写个例子测试下storm安装是否成功。当然选择了经典的代码storm-starter-master
tags: [storm, mysql, mybatis]
---

{{ page.title }}
================

{{page.summary}}

开发环境：

- idea
- jdk 1.6  
- maven
- git 

## 下载 strom-starter-master

运行 `git clone https://github.com/nathanmarz/storm-starter.git`，提示：

{% highlight text %}
localhost:log likunran$ git clone https://github.com/nathanmarz/storm-starter.gi
Cloning into 'storm-starter.gi'...
remote: Repository not found.
fatal: repository 'https://github.com/nathanmarz/storm-starter.gi/' not found
localhost:log likunran$ git clone https://github.com/nathanmarz/storm-starter.git
Cloning into 'storm-starter'...
remote: Counting objects: 756, done.
remote: Total 756 (delta 0), reused 0 (delta 0)
Receiving objects: 100% (756/756), 171.81 KiB | 117.00 KiB/s, done.
Resolving deltas: 100% (274/274), done.
Checking connectivity... done.
{% endhighlight %}

##maven 安装
运行 ｀maven' 命令查看 maven是否安装

{% highlight text %}
localhost:log likunran$ maven
-bash: maven: command not found
{% endhighlight %}

maven 没有安装，下载maven 配置环境变量

>./etc/profile 文件   全局共有配置，无论哪个用户登录，都会读取此> 文件
>/etc/bashrc    （一般在这个文件中添加系统级环境变量）全局（公有）配置，bash shell执行时，不管是何种方式，都会读取此文件。
>~/.bash_profile  （一般在这个文件中添加用户级环境变量）

添加环境变量
>MAVEN_HOME=/Users/likunran/tools/apache-maven-2.2.1
  >PATH=$MAVEN_HOME/bin:$PATH
  >export MAVEN_HOME
  >export PATH
  
 `source .bash_profile` 



##项目编译
   1.进入到storm-starter 文件夹 把 m2-pom.xml重命
   >mv m2-pom.xml pom.xml</br>
   >mvn install

   编译结果

{% highlight text %}
[INFO] BUILD SUCCESSFUL
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 9 seconds
[INFO] Finished at: Tue Sep 23 23:36:05 CST 2014
[INFO] Final Memory: 40M/85M
{% endhighlight %}

编译过程中请保持外网是可以正常访问的

##部署 到storm
1.进入 target目录看到storm-starter-0.0.1-SNAPSHOT-jar-with-dependencies.jar 文件已经存在。把文件部署到 storm

2.提交 ` ./storm jar /export/data/storm/storm-starter-0.0.1-SNAPSHOT-jar-with-dependencies.jar storm.starter.WordCountTopology wordcountTop －100`

3.访问storm ui
 

http://10.37.129.3:8080/
图如下
<img src="/css/images/st2.png" alt="Alt text" height="300">

#总结
有时候打包提交 会报错 Storm Found multiple defaults.yaml resources 。遇到这错误是因为storm的storm的defaultsyarm打进去了。这个文件在storm-core.jar里面已经有了，设置storm依赖的scope为provided好了
