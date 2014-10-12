---
layout: post
title: 设计模式－单例
category: 设计模式
summary: Singleton（单例）模式设计模式。由于设计模式在面向对象程序设计中有这举足轻重单作用。一些公司面试过程中很多公司都喜欢问一些设计模式相关问题。Singleton 是唯一一个能够用短短几十行代码完成都设计模式。因此singleton的类型的是一个很常见的面试题。
tags: [设计模式，单例]
urlId: 20141013
---

{{ page.title }}
================

{{page.summary}}


##题目：设计一个类 我们只能生成一个实例
  
### 解法一

  {% highlight text %}

public class SingletonOffer {

    private SingletonOffer() {

    }

    private static SingletonOffer _instance = null;

    public static SingletonOffer instance() {
        if (null == _instance) {
            _instance = new SingletonOffer();
        }
        return _instance;

    }

    public static void main(String[] args) {
        System.out.print(SingletonOffer.instance());
    }

}

{% endhighlight %}

##评点
	
   SingletonOffer 把构造函数设为私有变量禁止他人创建。 只能静态 instance() 方法 创建SingletonOffer 实例。只有_instance 为null时才会创建实例。但是在多线程下由于并非问题会出现实例多个_instance.
   

###优化后

{% highlight text %}

      public static SingletonOffer instance() {
        synchronized (_instance) {
            if (null == _instance) {

                _instance = new SingletonOffer();
            }
        }
        return _instance;

    }
    
{% endhighlight %}
  由于同一个时间只能有一个线程得到同步锁。当一个线程持有锁时第二个线程等待。第二个线程持有时就判断_instance 已经被实例化。这样就多线程多问题。但是每一次都需要锁比较消耗性能。客户进一步优化代码



{% highlight text %}
   public static SingletonOffer instance() {

        if (null == _instance) {
            synchronized (_instance) {
                if (null == _instance) {

                    _instance = new SingletonOffer();
                }
            }
        }
        return _instance;

    }
{% endhighlight %}
_instace 为null时加锁。当_instace已经创建出来后。就不需要加锁了。


## 静态构造函数


{% highlight text %}
 static SingletonOfferLock _instance =new SingletonOfferLock();
    private SingletonOfferLock(){

    }

    public static SingletonOfferLock instance(){
        return _instance;
    }

    public static void main(String[] args) {
        System.out.print(SingletonOfferLock.instance());
    }
{% endhighlight %}
  使用静态变量初始化_instance代码非常简洁。静态变量会在程序程序初始化时创建一个实例。 但是这个实例是系统控制的。无论需要不需要第一次调用SingletonOfferLock都会被创建。实例创建过早会降低内存的使用效率。
 

##优化


{% highlight text %}
public class SingletonOfferLock {


    private SingletonOfferLock(){

    }

    public static SingletonOfferLock instance(){
        return Nested.instance;
    }

    public static void main(String[] args) {
        System.out.print(SingletonOfferLock.instance());
    }

   static class Nested{
       private  Nested(){

       }

       static final SingletonOfferLock instance = new SingletonOfferLock();
   }

}
{% endhighlight %}

由于Nested是私有变量 别人无法使用Nested类型。因此只有我们第一次试图通过属性创建 instance 时。对象才被实例化。
