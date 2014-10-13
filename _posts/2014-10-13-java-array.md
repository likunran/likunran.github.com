---
layout: post
title: 二维数组中的查找
category: 集合
summary: 数组是有序数据的集合，数组中的每个元素具有相同的数组名和下标来唯一地确定数组中的元素。数组是最简单的数据结构，它占据了一块连续的内存并按照顺序存储。创建数组时我们首先指定容量大小，然后根据大小分配内存。因此空间效率不好，经常有空闲区域没有得到充分利用。由于数据中的内存时连续的。于是可以根据下标读写任何元素。因此他的效率很高。
tags: [设计模式，单例]
urlId: 2014101322
---

{{ page.title }}
================

{{page.summary}}


##题目：
 * 在一个二维数组中。每一行都按照从左到右递增顺序排序。
 * 每一列都按照从上到下递增的顺序排序。请完成一个函数。输出这样的一 个二维数组和一个整数
 * 判断数组中是否含有该整数
  
### 解法一

  {% highlight text %}

      private static int[][] data = {
            {1, 2, 8, 9},
            {2, 4, 9, 12},
            {4, 7, 10, 13},
            {6, 8, 11, 15}};

    /**
     * 查找int 值是否在数组中存在
     *
     * @param data
     * @param find
     */
    private static boolean findByEach(int[][] data, int find) {

        if (data.length < 0) {
            System.out.print("二维数组空。查找数字不存在");
            return false;
        }

        int count = 0;

        for (int i = 0; i < data.length; i++) {
            count++;
            for (int j = 0; j < data[i].length; j++) {
                count++;
                if (data[i][j] == find) {
                    System.out.print(String.format("find success count[%s]", count));
                    return true;
                }
            }

        }

        System.out.print(String.format("find end count[%s]", count));

        return false;

    }


      public static void main(String[] args) {

        System.out.println(findByEach(data, 0));
        System.out.println(findByEach(data, 7));
    }

{% endhighlight %}
执行查找方法 发现存在的时候执行了。13次。没有查找到时查找了 20次。有没有更好到办法呢。数据时递增的？可以根据递增的特性优化。
｀ find end count[20]false
   find success count[13]true
｀
   

###优化后

{% highlight text %}

     private static int[][] data = {
            {1, 2, 8, 9},
            {2, 4, 9, 12},
            {4, 7, 10, 13},
            {6, 8, 11, 15}};

      private static boolean findByArea(int[][] data, int find) {

        if (data.length < 0) {
            System.out.print("二维数组空。查找数字不存在");
            return false;
        }

        int count = 0;

        for (int i = 0; i < data.length; i++) {
            count++;
            if (data[i][0] < find) {
                for (int j = 0; j < data[i].length; j++) {
                    count++;
                    if (data[i][j] <= find) {
                        if (data[i][j] == find) {
                            System.out.print(String.format("find success [%s]", count));
                            return true;

                        }
                    } else {
                        break;
                    }
                }
            } else {
                break;
            }

        }

        //由于递增 所以可以根据递增判断查找大小

        System.out.print(String.format("find end count[%s]", count));
        return false;
    }

    public static void main(String[] args) {

        System.out.println(findByArea(data, 0));
        System.out.println(findByArea(data, 7));
    }
    
{% endhighlight %}
查找相同的数据。可以找到时执行11次。找不到时执行1次。
 ｀
 find end count[1]false
 find success [11]true
｀
 

##总结
  当遇到需要解决一个复杂问题时。一个很有效当办法就是从一个具体当问题入手。通过分析简单具体当例子。试图寻找普遍的规律。

