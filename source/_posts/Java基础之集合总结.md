---
title: Java基础之集合总结
copyright: true
comments: true
date: 2019-08-07 16:53:01
tags:
 - 集合
 - Java基础
 - collection
categories: Java基础
description: 本文主要是根据 Java 中的集合相关知识点进行部分面试题的总结。
sticky:
---
# 集合

&emsp;&emsp;在Java编程语言中的容器主要是Array（数组）和collection（集合），下面我们就来看看Java中的容器：

## 集合和数组
 * 数组（可以存储`基本数据类型`和`引用数据类型`）是用来存储对象的一种容器，但是数组的长度固定不变的，不适合在对象数量未知的情况下使用。

 * 集合（只能`引用数据类型`对象，对象类型可以不一样）的长度可变，可在多数情况下使用。

* 注意：虽然集合不能存储基本数据类型，但是可以存储基本数据类型的包装类类型。

## 集合框架层次关系

&emsp;&emsp;这里小编就不介绍数组了，本文主要讲解集合的知识点总结。如下图所示：在图中，实线边框的是实现类，折线边框的是抽象类，点线边框的是接口。
![集合框架图.png](集合框架图.png)

* 说明：
    Collection接口是集合类的根接口，Java中没有提供这个接口的直接的实现类。但是有两个子接口，就是Set和List。Set集合中不能包含重复的元素。List集合是一个有序的集合，可重复元素，提供了按索引访问元素的方式。

    Map是Java.util包中的另一个接口，注意，它和我们前面说的Collection接口没有任何关系，是互相独立的，也是Java集合中的一部分。

    Map集合是以键值对的形式存储数据的，Map集合不能包含重复的key，一个key映射一个value。但是可以包含相同的value。

    Iterator 迭代器，Java中的所有的集合类都实现了该接口，这是一个用于遍历集合的接口，该接口中主要包含三个方法，源码如下：
    ```java
    public interface Iterator<E> {
        //是否还有下一个元素
        boolean hasNext();

        //返回下一个元素
        E next();

        //删除当前元素
        default void remove() {
        throw new UnsupportedOperationException("remove");
        }
    }
    ```

## 集合家族的体系介绍

 * 1、集合分类
    * 单列集合：每个元素都是一个独立的个体，存储的都是单身狗。
    * 双列集合：每个元素都是针对一对数据进行的，一对数据才是一个存储单元，存储的都是一对一对的小情侣。
 * 2、单列集合的体系结构：
    * Collection    ---->   单列集合顶层接口
        * List      ---->   有序的子接口
            * ArrayList     ---->   顺序存储，查改快，增删慢
            * LinkedList    ---->   链式存储，增删快，查改慢
            * vector        ---->   顺序存储，各种操作慢
        * Set       ---->   无序且元素唯一子接口
            * HashSet       ---->   哈希存储
                * LinkedHashSet     ---->   是HashSet的子类, 哈希加链表的方式存储
 * 3、双列集合的体系 
    * Map           ---->   双列集合的顶层接口
        * HashMap   ---->   哈希表存储
            * LinkedHashMap     ---->   是HashMap的子类, 哈希加链表的方式存储

## 整体总结

### 1、Java集合框架是什么？说出一些集合框架的优点？

&emsp;&emsp;每种编程语言中都有集合，最初的Java版本包含几种集合类：Vector、Stack、HashTable和Array。随着集合的广泛使用，Java1.2提出了囊括所有集合接口、实现和算法的集合框架。在保证线程安全的情况下使用泛型和并发集合类，Java已经经历了很久。它还包括在Java并发包中，阻塞接口以及它们的实现。集合框架的部分优点如下：

 * （1）使用核心集合类降低开发成本，而非实现我们自己的集合类。
 * （2）随着使用经过严格测试的集合框架类，代码质量会得到提高。
 * （3）通过使用JDK附带的集合类，可以降低代码维护成本。
 * （4）复用性和可操作性。

### 2、集合框架中的泛型有什么优点？

&emsp;&emsp;Java1.5引入了泛型，所有的集合接口和实现都大量地使用它。泛型允许我们为集合提供一个可以容纳的对象类型，因此，如果你添加其它类型的任何元素，它会在编译时报错。这避免了在运行时出现ClassCastException，因为你将会在编译时得到报错信息。泛型也使得代码整洁，我们不需要使用显式转换和instanceOf操作符。它也给运行时带来好处，因为不会产生类型检查的字节码指令。

### 3、Java集合框架的基础接口有哪些？

&emsp;&emsp;Collection为集合层级的根接口。一个集合代表一组对象，这些对象即为它的元素。Java平台不提供这个接口任何直接的实现。

&emsp;&emsp;Set是一个不能包含重复元素的集合。这个接口对数学集合抽象进行建模，被用来代表集合，就如一副牌。

&emsp;&emsp;List是一个有序集合，可以包含重复元素。你可以通过它的索引来访问任何元素。List更像长度动态变换的数组。

&emsp;&emsp;Map是一个将key映射到value的对象.一个Map不能包含重复的key：每个key最多只能映射一个value。

一些其它的接口有Queue、Dequeue、SortedSet、SortedMap和ListIterator。

### 4、为何Collection不从Cloneable和Serializable接口继承？

&emsp;&emsp;Collection接口指定一组对象，对象即为它的元素。如何维护这些元素由Collection的具体实现决定。例如，一些如List的Collection实现允许重复的元素，而其它的如Set就不允许。很多Collection实现有一个公有的clone方法。然而，把它放到集合的所有实现中也是没有意义的。这是因为Collection是一个抽象表现。重要的是实现。

&emsp;&emsp;当与具体实现打交道的时候，克隆或序列化的语义和含义才发挥作用。所以，具体实现应该决定如何对它进行克隆或序列化，或它是否可以被克隆或序列化。

&emsp;&emsp;在所有的实现中授权克隆和序列化，最终导致更少的灵活性和更多的限制。特定的实现应该决定它是否可以被克隆和序列化。

### 5、为何Map接口不继承Collection接口？

&emsp;&emsp;尽管Map接口和它的实现也是集合框架的一部分，但Map不是集合，集合也不是Map。因此，Map继承Collection毫无意义，反之亦然。

&emsp;&emsp;如果Map继承Collection接口，那么元素去哪儿？Map包含key-value对，它提供抽取key或value列表集合的方法，但是它不适合“一组对象”规范。

### 6、Iterator是什么？

&emsp;&emsp;Iterator接口提供遍历任何Collection的接口。我们可以从一个Collection中使用迭代器方法来获取迭代器实例。迭代器取代了Java集合框架中的Enumeration。迭代器允许调用者在迭代过程中移除元素。