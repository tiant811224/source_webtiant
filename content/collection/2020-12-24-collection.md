---
title: "【转载】Java 容器"
date: 2020-12-24T16:47:24+08:00
author: "糸色先生"
categories: [
    "Java"
]
---
参考自 [CyC2018/CS-Notes](https://github.com/CyC2018/CS-Notes/blob/master/notes/Java%20%E5%AE%B9%E5%99%A8.md)

* [一、概览](#1)
    * [Collection](#1.1)
    * [Map](#1.2)
<!-- * [二、容器中的设计模式]
    * [迭代器模式](#2.1)
    * [适配器模式](#2.2)
* [三、源码分析](#3)
    * [ArrayList](#3.1)
    * [Vector](#3.2)
    * [CopyOnWriteArrayList](#3.3)
    * [LinkedList](#3.4)
    * [HashMap](#3.5)
    * [ConcurrentHashMap](#3.6)
    * [LinkedHashMap](#3.7)
    * [WeakHashMap](#3.8) -->

### 一、概览 <a name="1"></a>
容器主要包括 Collection 和 Map 两种，Collection 存储着对象的集合，而 Map存储着键值对（两个对象）的映射表。
#### Collection <a name="1.1"></a>
**1.Set**  

* TreeSet: 基于红黑树实现，支持有序性操作，例如根据一个范围查找元素的操作。但是查找效率不如 HashSet, HashSet 查找的时间复杂度为 O(1)，TreeSet 则为 O(logN)。
* HashSet: 基于哈希表的实现，支持快速查找，但不支持有序性操作。并且失去了元素的插入顺序信息，也就是说使用 Iterator 遍历 HashSet 得到的结果是不确定的。
* LinkedHashSet: 具有 HashSet 的查找效率，并且内部使用双向链表维护元素的插入顺序。

**2.List**  
* ArrayList： 基于动态数组实现，支持随机访问。
* Vector： 和 ArrayList 类似，但它是线程安全的。
* LinkedList： 基于双向链表实现，只能顺序访问，但是可以快速地在链表中间插入和删除元素。不仅如此，LinkedList 还可以用做栈、队列和双向队列。  

**3.Queue**  
* LinkedList：可以用它来实现双向队列。
* PriorityQueue：基于堆结构实现，可以用它来实现优先队列。

#### Map <a name="1.2"></a>
* TreeMap：基于红黑树实现
* HashMap：基于哈希表实现
* hashTable：和 HashMap 类似，但它是线程安全的，这意味着同一时刻多个线程同时写入 HashTable 不会导致数据不一致。它是遗留类，不应该去使用它，而是使用 ConcurrentHashMap 来支持线程安全，ConcurrentHashMap 的效率会更高，因为 ConcurrentHashMap 引入了分段锁。
* LinkedHashMap：使用双向链表来维护元素的顺序，顺序为插入顺序或者最近最少使用（LRU）顺序。