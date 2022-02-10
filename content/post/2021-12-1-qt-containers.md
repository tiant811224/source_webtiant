---
title: "【转载】Qt6 容器类-概述"
date: 2021-12-01T22:05:26+08:00
series: ["Qt 容器类"] 
author: "糸色生"
slug: "qt-containers"

categories: [
    "Qt"
]

---

### 介绍

Qt 库提供了一组通用的基于模板的容器类。这些类可用于存储制定类型的项。例如，您需要一个可调整大小的 QString 数组，请使用 QList<QString>。

这些容器类被设计为比 STL 容器更轻、更安全且更易于使用。如果您不熟悉 STL，或者更喜欢“Qt 方式”实现，您可以使用这些类而不是 STL 类。

容器类是 [隐式共享的](/post/2021/12/01/implicit-sharing/)，它们是可重入的，并且它们针对速度、低内存消耗和最小的内联代码扩展进行了优化，从而产生更小的可执行文件。此外，在所有用于访问它们的线程将它们用作只读容器的情况下，它们是线程安全的。

容器提供了遍历的迭代器。STL 样式的迭代器是最有效的迭代器，可以与 Qt 和 STL 的通用算法一起使用。提供 Java 风格的迭代器是为了向后兼容。

### 容器类

Qt 提供以下顺序容器：QList、QStack 和 QQueue。对于大多数应用程序，QList 是最好的类型。它提供了非常快速的追加。如果您确实需要链表，请使用 std::list。QStack 和 QQueue 是提供 LIFO 和 FIFO 语义的便利类。QList和QVector在 Qt 6 中被统一。两者都使用来自QVector的数据模型。QVector现在是QList的别名。

Qt 还提供了这些关联容器：QMap、QMultiMap、QHash、QMultiHash和QSet。“Multi”容器方便地支持与单个键关联的多个值。“散列”容器通过使用散列函数而不是对排序集的二分搜索来提供更快的查找。

作为特殊情况，QCache 和 QContiguousCache 类在有限的缓存存储中提供了有效的对象散列查找。

|  类   | 简述  |
|  ----  | ----  |
| QList <T>  | 这是迄今为止最常用的容器类。它存储可以通过索引访问的给定类型 (T) 的值列表。在内部，它在内存中的相邻位置存储一组给定类型的值。在列表的前面或中间插入可能会很慢，因为它可能导致大量项目必须在内存中移动一个位置。 |
| QVarLengthArray <T, Prealloc >  | 这提供了一个低级的可变长度数组。在速度特别重要的地方，它可以代替QList使用。 |
| QStack <T>  | 这是QList 的一个便利子类，提供“后进先出”(LIFO) 语义。它向QList 中已有的函数添加了以下函数：push ()、pop () 和top ()。 |
| QQueue <T>  | 这是QList的便利子类，提供“先进先出”(FIFO) 语义。它将以下函数添加到QList 中已经存在的函数中：enqueue ()、dequeue () 和head ()。 |
| QSet <T>  | 这提供了具有快速查找功能的单值数学集。 |
| QMap <Key, T>  | 这提供了一个字典（关联数组），将 Key 类型的键映射到 T 类型的值。通常每个键都与单个值相关联。QMap以 Key 顺序存储其数据；如果顺序无关紧要QHash是一个更快的选择。 |
| QMultiMap <Key, T>  | 这是QMap 的一个方便的子类，它为多值映射提供了一个很好的接口，即一个键可以与多个值相关联的映射。 |
| QHash <Key, T>  | 这与QMap具有几乎相同的 API ，但提供了明显更快的查找。QHash以任意顺序存储其数据。 |
| QMultiHash <Key, T>  | 这是QHash 的一个便利子类，它为多值散列提供了一个很好的接口。 |

### 算法复杂性

下表总结了顺序容器 QList 的算法复杂度。

|     | 索引查找  | 插入  | 前置  | 附加  |
|  ----  | ----  | ----  | ----  | ----  |
| QList <T>  | O(1) | O(n) | O(n) | Amort.O(1) |

> 在表中，“Amort.”代表“摊销行为”。例如，“Amort.O(1)”意味着如果你只调用一次函数，你可能会得到 O(n) 的行为，但如果你多次调用它（例如，n 次），平均行为将是 O(1)。

下表总结了 Qt 的关联容器和集合的算法复杂度：

| | 键查找-平均数 | 键查找-最差的情况 | 插入-平均数 | 插入-最差的情况 |
|  ----  | ----  | ----  | ----  | ----  |
| QMap<Key, T> | O(log n ) | O(log n )	| O(log n )	| O(log n ) |
| QMultiMap<Key, T>	| O(log n )	| O(log n )	| O(log n )	| O(log n ) |
| QHash<Key, T> | Amort.O(1) | O( n ) | Amort.O(1) | O( n ) |
| QSet<Key> | Amort.O(1) | O( n ) | Amort.O(1) | O( n ) |