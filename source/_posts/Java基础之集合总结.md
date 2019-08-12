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
description: 本文主要概述Java基础中的集合框架的知识点总结。
sticky:
---
# 集合

&emsp;&emsp;在Java编程语言中的容器主要是Array（数组）和collection（集合），下面我们就来看看Java中的容器：

## 集合和数组
 * 数组（可以存储`基本数据类型`和`引用数据类型`）是用来存储对象的一种容器，但是数组的长度固定不变的，不适合在对象数量未知的情况下使用。

 * 集合（只能存储`引用数据类型`对象，存储的对象类型可以是不同类型的）的长度可变，可在多数情况下使用。

* 注意：虽然集合不能存储基本数据类型，但是可以存储基本数据类型的包装类类型。

## 集合框架层次关系

&emsp;&emsp;这里小编就不介绍数组了，本文主要讲解集合的知识点总结。如下图所示：在图中，实线边框的是实现类，折线边框的是抽象类，点线边框的是接口。
![集合框架图.png](集合框架图.png)

* 说明：
    &emsp;&emsp;Collection接口是集合类的根接口，Java中没有提供这个接口的直接的实现类。但是有两个子接口，就是Set和List。Set集合中不能包含重复的元素。List集合是一个有序的集合，可重复元素，提供了按索引访问元素的方式。

    &emsp;&emsp;Map是Java.util包中的另一个接口，注意，它和我们前面说的Collection接口没有任何关系，是互相独立的，也是Java集合中的一部分。

    &emsp;&emsp;Map集合是以键值对的形式存储数据的，Map集合不能包含重复的key，一个key映射一个value。但是可以包含相同的value。

    &emsp;&emsp;Iterator 迭代器，Java中的所有的集合类都实现了该接口，这是一个用于遍历集合的接口，该接口中主要包含三个方法，源码如下：
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

# 整体总结

### 集合（Collection）
&emsp;&emsp;Collection 的特点：
- 部分集合是有序的，部分集合是无序的，这里的有序指的是存储有序
- 部分集合是可排序的，部分集合是不可排序
- 部分集合是可重复的，部分集合是不可重复，唯一的

## 单列集合

### List接口
List接口
 - 特点: 
  - 1.集合是有序的(存储有序)
  - 2.有索引，方便查找和修改
  - 3.List集合可重复

  ```java
  public boolean add(E e) {
	ensureCapacityInternal(size + 1);  // Increments modCount!!
	elementData[size++] = e;
	return true;
  }
  ```
  `结论`:无论添加重复还是不重复的元素，结果都是返回添加成功，所以List集合是可重复的
  - 4.允许存储null值

#### ArrayList
 - ArrayList集合的特点：
	1.有序，有索引
	2.元素可以重复
	3.可以存储null值
	4.随机访问速度快，修改快，增加/插入或者移除/删除的效率慢
	5.线程不安全

 - 如何去重复？
	1.创建一个新集合
	2.选择排序思想去重复

 - 如何排序?
 	Collections.sort(list);

 - 如何变安全?
	Collections.synchronizedList(list);

 - 遍历的方式（六种）:
	1、Object[] toArray() 
	2、普通for循环
	3、增强for循环
	4、Iterator<E> iterator() 
	5、ListIterator<E> listIterator() 
	6、<T> T[] toArray(T[] a) 

 - ListIterator 列表迭代器
 &emsp;&emsp;ListIterator接口是List特有的迭代器，允许程序员按任一方向遍历列表、迭代期间修改列表，并获得迭代器在列表中的当前位置。

  - 常用方法 
	`boolean hasNext() `
	以正向遍历列表时，如果列表迭代器有多个元素，则返回 true（换句话说，如果next 返回一个元素而不是抛出异常，则返回 true）。 

	`E next()`       
	返回列表中的下一个元素。 

	`boolean hasPrevious()` 
	如果以逆向遍历列表，列表迭代器有多个元素，则返回 true。 

	`E previous()`   
	返回列表中的前一个元素。

#### Vector
&emsp;&emsp;该集合类似于ArrayList集合，可以说成老版的ArrayList，该集合在jdk1.0版本时就有了。

&emsp;&emsp;Vector集合底层数据结构也是数组，增加、删除元素的性能就比较差了，相对于ArrayList是线程安全的、效率低。

 - Vector类特有功能
 ```java
 public void addElement(E obj)
 public E elementAt(int index)
 public Enumeration elements()
 ```

#### LinkedList
 - LinkedList类概述
	1、底层数据结构是链表
	2、插入和移除性能高，查询效率低。
	3、线程不安全，效率高。
	4、存储有序
	5、可以重复（并且可以存储null值）
	6、不可排序

&emsp;&emsp;双链表实现了List和Deque以及Queue接口。 实现所有可选列表操作，并允许所有元素（包括null ）。
&emsp;&emsp;该集合具有队列、栈、链表的数据特性。可以使用LinkedList实现栈和队列结构。

#### Stack

	底层数据结构是使用的栈结构，先进后出的形式。

	方法:

    ```java
    boolean empty()  测试堆栈是否为空。 
    E peek()  查看堆栈顶部的对象，但不从堆栈中移除它。 
    E pop()   移除堆栈顶部的对象，并作为此函数的值返回该对象。 
    E push(E item)  把项压入堆栈顶部。 
    int search(Object o)   返回对象在堆栈中的位置，以 1 为基数。        
    ```

	java.util.EmptyStackException

	EmptyStackException: 空栈异常

	产生原因: 栈里面的数据没有了

	解决办法: 在弹栈之前做判断

### Set接口

 - Set集合的概述

  - 1.Set是一个不包含重复元素的 Collection的另一子接口
  - 2.特点:

    - 无序: 没有任何的前后区别.所有的元素没有位置的概念,所有的元素都存储在集合中.无序指的你添加顺序和取出顺序可能不一样.取出顺序的不可预知.
    - 没有索引: 集合中元素无法通过位置区分,相同元素就无法区分.
    - 唯一(不能重复):没有位置区分, 相同的元素就无法分别, 所以不能重复.
    - 可以存储null值，但只能存储一个null值。

  - 3.Set的是一个接口, 无法实例化,只能指向HashSet实现类才能研究Set集合中的方法:HastSet: 就是Set的实现类, 底层是使用哈希表存储方法存储Set集合的元素.

  - 4.存储特点:
    - 1.相同的元素无法存储进Set集合,相同只能保存一份;
    - 2.集合本身不保证顺序: 存储的顺序和取出顺序顺序不一致.

#### HashSet
 - HashSet类概述 
&emsp;&emsp;此类实现 Set 接口，由哈希表（实际上是一个 HashMap 实例）支持。它不保证 set 的迭代顺序；特别是它不保证该顺序恒久不变。此类允许使用null元素。HashSet保证元素唯一。 

&emsp;&emsp;底层数据结构是哈希表，哈希表依赖于哈希值存储 

&emsp;&emsp;添加元素时保证元素唯一，本质底层依赖两个方法:

    ```java
    int hashCode() 

    boolean equals(Object obj)
    ```

 - HashSet保证元素唯一的原理
  - 1.某个对象,在即将存储到HashSet集合时候,首先获取obj的hashCode方法值
  - 2.在集合中与每个元素的哈希值比较, 如果都和obj的哈希值不同, 说明obj在集合中不存在,那么直接添加obj.
  - 3.在集合中有若干元素的哈希值和obj哈希值相同，但是此时不能说明obj已经存在于集合中了，这时我们需要使用equals判断obj是否和自己哈希值相同元素是否相等.
  - 4.如果obj和所有相同哈希值一样的元素比较之后返回值都是false.那么就说明obj不存在于集合中,可以将obj添加到集合中.
  - 5.如果obj和其中某个相同哈希值元素比较的结果为true.那么就说明obj已经存在于集合中. obj就不能向集合中添加.

 - 自定义类型如何保证HashSet存储时元素的唯一性
  - 1.重写hashCode方法
    相同对象,一定有相同的哈希值
    不同对象,尽量有不同哈希值, 为了提供哈希表存储效率.
    重写都是围绕着对象的属性重写;
  - 2.重写equals方法:
    不仅仅比较两个对象地址,还要两个对象属性.
  - 3.重写操作:
    使用快捷键: alt+shift+s  h

#### LinkedHashSet

 - 1.LinkedHashSet是HashSet一个子类, 和HashSet一样都能保证元素的唯一性.
 - 2.底层存储原理和HashSet不一样.底层使用哈希表+链接列表方法存储数据.
	哈希表保证元素的唯一.
	链表用来记录元素存储的先后顺序.
	链表通过前一个元素,记录后一个元素位置，来维护它有序性.
 - 3.存储效果:
    元素具有可预知迭代顺序,元素添加顺序和取出的顺序一致.
 - 4.LinkedHashSet的应用场景:
    如果你想使用一个集合,既保证元素唯一性,又想保证元素的存储顺序,就使用LinkedHashSet.

#### TreeSet
&emsp;&emsp;基于 TreeMap 的 NavigableSet 实现。使用元素的自然顺序对元素进行排序，或者根据创建 set 时提供的 Comparator 进行排序，具体取决于使用的构造方法。 
 - 排序方式完全取决于使用的构造方法。
  - 1、创建TreeSet集合是使用的是无参构造器时，Comparator为null ，则底层使用的是自然排序法。
  ```java
    public TreeSet() {
        this(new TreeMap<E,Object>());
    }

    public TreeMap() {
        comparator = null;
    }
  ```

  - 2、创建TreeSet集合时使用的是有参构造器时，Comparator不为null，则底层使用的时比较器排序
  ```java
    public TreeSet(Comparator<? super E> comparator) {
        this(new TreeMap<>(comparator));
    }

    public TreeMap(Comparator<? super K> comparator) {
        this.comparator = comparator;
    }
  ```

&emsp;&emsp;TreeSet保证元素的排序和唯一性的:底层数据结构是红黑树(自平衡的二叉树)

&emsp;&emsp;一般情况下大部分系统类都重写了Comparable接口，而我们自定义的类如果要使用TreeSet集合时，并且使用自然排序法时，该类必须实现Comparable接口并实现该接口中的方法自定义自己的排序规则。

&emsp;&emsp;`注意`：如果我们同时实现了自然排序和比较器排序，那么比较器排序优先于自然排序。我们在开发中一般使用比较器排序，并且使用匿名内部类的方式实现。

## 双列集合

### Map接口
&emsp;&emsp;Map是双列集合的顶层接口

 - Map集合的特点:
  - key(键值): 在map中唯一,不可重复

  - value(值): 在map中不要求唯一, 可以多个key值对应相同value值.  

  - key和value: 就称之为键值对

  - 每个key值只能对应一个value值.

 - Map和Collection区别
  - 1.Map是双列集合, Collection是单列集合
  - 2.Map的键值唯一的, Collection下Set接口实现类存储元素是唯一的
  - 3.Map集合所有操作和算法都是针对于key值有效.Set集合下所有操作和算法针对于元素有效.原因是Set的底层就是Map.

#### HashMap
&emsp;&emsp;基于哈希表的 Map 接口的实现。此实现提供所有可选的映射操作，并允许使用 null 值和 null 键。（除了非同步和允许使用 null 之外，HashMap 类与 Hashtable 大致相同。）

&emsp;&emsp;此类不保证映射的顺序，特别是它不保证该顺序恒久不变。

 - 特点：
  - 键无序，唯一，类似于Set集合
  - 值有序，可重复，类似于List
  - 底层数据结构是哈希表，保证键唯一
  - 允许键为null，值为null

&emsp;&emsp;HashMap存储自定义类型作为key时,无法保证元素的唯一性. 原因是HashMap的底层是使用的是哈希算法来保证元素的唯一性。

 - 如何保证自定类型作为key时唯一: 重写equals和hashCode方法.
 - 说明: HashMap保证key值唯一性的原理和HashSet保证元素唯一的原理一样.

 - HashMap和HashSet的关系:
  - 1.HashSet是由HashMap实现的, HashSet就是HashMap的key值那一列;
  - 2.HashSet的内部隐藏了HashMap的value值那一列. HashSet没有提供操作value值那一列方法.

#### Hashtable
&emsp;&emsp;此类实现一个哈希表，该哈希表将键映射到相应的值。任何非 null 对象都可以用作键或值
 - 特点：
	不允许null键和null值
	线程安全，效率低

 - 面试题：HashMap和Hashtable的区别
	HashMap是不安全的不同步的效率高的  允许null键和null值
	Hashtable是安全的同步的效率低的 不允许null键和null值
	底层都是哈希表结构

#### LinkedHashMap
&emsp;&emsp;LinkedHashMap 是 HashMap 接口的一个子类

 -  特点：
	键有序，唯一，
	值有序，可重复，类似于List
	底层数据结构是哈希表和链表，哈希表保证键唯一，链表保证键有序

#### TreeMap
&emsp;&emsp;基于红黑树（Red-Black tree）的 NavigableMap 实现。该映射根据其键的自然顺序进行排序，或者根据创建映射时提供的 Comparator 进行排序，具体取决于使用的构造方法。

 - 特点：
	键可排序，唯一，
	值有序，可重复，类似于List
	底层数据结构是自平衡的二叉树，可排序
	排序方式类似于TreeSet，分为自然排序和比较器排序，具体取决于使用的构造方法

#### WeakHashMap
&emsp;&emsp;以弱键 实现的基于哈希表的 Map。在 WeakHashMap 中，当某个键不再正常使用时，将自动移除其条目。更精确地说，对于一个给定的键，其映射的存在并不阻止垃圾回收器对该键的丢弃，这就使该键成为可终止的，被终止，然后被回收。丢弃某个键时，其条目从映射中有效地移除，因此，该类的行为与其他的 Map 实现有所不同。

## Iterator接口
&emsp;&emsp;对 collection 进行迭代的迭代器。迭代器取代了 Java Collections Framework 中的 Enumeration，迭代器依赖于集合而存在。

 - 常用方法：
 ```java
 boolean hasNext() 
       如果仍有元素可以迭代，则返回 true。 

 E next() 
       返回迭代的下一个元素。 

注意： 
       抛出： NoSuchElementException - 没有元素可以迭代。 

void remove() 

       返回迭代的当前元素。 
 ```

 - 迭代器原理：
	在获取迭代器的时候，会创建一个集合的副本，同时创建一个指针指向迭代器集合的起始位置。

## HashMap和Hashtable的区别
 - 1、HashMap是Hashtable的轻量级实现（非线程安全的实现），他们都完成了Map接口，主要区别在于HashMap允许空（null）键值（key）,由于非线程安全，效率上可能高于Hashtable。

 - 2、HashMap允许将null作为一个entry的key或者value，而Hashtable不允许。

 - 3、HashMap把Hashtable的contains方法去掉了，改成containsvalue和containsKey。因为contains方法容易让人引起误解。

 - 4、Hashtable继承自Dictionary类,jdk1.0时出现的，而HashMap是Java1.2引进的Map interface的一个实现。

 - 5、最大的不同是，Hashtable的方法是Synchronize的，而HashMap不是，在多个线程访问Hashtable时，不需要自己为它的方法实现同步，而HashMap 就必须为之提供外同步。 

 - 6、Hashtable和HashMap采用的hash/rehash算法都大概一样，所以性能不会有很大的差异。

 - 相同点：都实现Map接口，都是KEY-VALUE，所用的算法差不多

 - 不同点：
  - HashMap:允许将null作为一个entry的key或者value，线程不安全，效率高一些
  - Hashtable:不允许将null作为一个entry的key或者value，线程安全，效率低一些

## 请说出集合框架中线程安全与不安全的类
 - 集合框架中的类
  - ArrayList 线程不安全
  - Vector 线程安全

  - HashMap 线程不安全
  - Hashtable 线程安全

 - 不是集合中的类
  - StringBuffer 线程安全的
  - StringBuilder 线程不安全的