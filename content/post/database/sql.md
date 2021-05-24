---
title: "SQL 语法"
date: 2020-12-31T10:12:40+08:00
author: "罗泽勋"
draft: true

tags: [

]
categories: [
    "数据库",
]
---

* [一、基础](#1)
* [二、创建表](#2)
* [三、修改表](#3)
* [四、插入](#4)
* [五、更新](#5)
* [六、删除](#6)
* [七、查询](#7)
    * [DISTINCT](#7.1)
    * [LIMIT](#7.2)
* [八、排序](#8)
* [九、过滤](#9)
* [十、通配符](#10)
* [十一、计算字段](#11)
* [十二、函数](#12)
    * [汇总](#12.1)
    * [文本处理](#12.2)
    * [日期和时间处理](#12.3)
    * [数值处理](#12.4)
* [十三、分组](#13)
* [十四、子查询](#14)
* [十五、连接](#15)
    * [内连接](#15.1)
    * [自连接](#15.2)
    * [自然连接](#15.3)
    * [外连接](#15.4)
* [十六、组合查询](#16)
* [十七、视图](#17)
* [十八、存储过程](#18)
* [十九、游标](#19)
* [二十、触发器](#20)
* [二十一、事务管理](#21)
* [二十二、字符集](#22)
* [二十三、权限管理](#23)

### 一、基础<a name="1"></a>
模式定义了数据如何存储、存储怎么样的数据以及数据如何分解等信息，数据库和表都有模式。  

主键的值不允许修改，也不允许复用（不能将已经删除的主键值赋给新数据行的主键）。  

SQL，（Structred Query Language)，标准 SQL 由 ANSI 标准委员会管理，从而被称为 ANSI SQL。各个 DBMS 都有自己的实现，如 PL/SQL、Transact-SQL 等。  

SQL 语句不区分大小写，但是数据库表名、列名和值是否区分依赖于具体的 DBMS 以及配置。  

SQL 支持以下三种注释：
```
### 注释
SELECT * FROM mytable; -- 注释
/* 注释1
   注释2 */

```

数据库创建与使用：
```
CREATE DATABASE test;
USE test;
```

### 二、创建表<a name="2"></a>
```
CREATE TABLE mytable (
    # int 类型，不为空，自增
    id INT NOT NULL AUTO_INCREMENT,
    # int 类型，不可为空，默认值为1，不为空
    col1 INT NOT NULL DEFAULT 1,
    # 变长字符串类型，最常为 45 个字符，可以为空
    col2 VARCHAR(45) NULL,
    # 日期类型，可为空
    col3 DATE NULL,
    # 设置主健为 id
    PRIMARY KEY (`id`)
);
```
 ### 三、修改表
添加列
```
ALTER TABLE mytable
ADD col CHAR(20);
```
删除列
```
ALTER TABLE mytable
DROP COLUMN col;
```
删除表
```
DROP TABLE mytable;
```

### 四、插入
普通插入
```
INSERT INTO mytable(col1, col2)
VALUES(val1,val2);
```

插入检索出来的数据
```
INSERT INTO mytable1(col1, col2)
SELECT col1, col2
FROM mytable2;
```

将一个表的内容插入到一个新表
```
CREATE TABLE newtable AS 
SELECT * FROM mytable;
```

### 五、更新
```
UPDATE mytable
SET col = val 
WHERE id = 1;
```

### 六、删除

```
DELETE FROM mytable
WHERE id = 1;
```

TRUNCATE TABLE table 可以清空表，也就是删除所有行。
```
TRUNCATE TABLE mytable;
```

使用更新和删除操作时一定要用 WHERE 字句，不然会把整张表的数据都损坏。用 SELECT 语句进行测试，防止错误删除。

### 七、查询
DISTINCT
相同值只会出现一次。它作用与所有列，也就是说所有列的值都想同时才算相同。
```
SELECT DISTINCT col1, col2
FROM mytable;
```

LIMIT
限制返回的行数。可以有两个参数，第一个参数为起始行，从 0 开始；第二个参数为返回的总行数。  
返回前五行：
```
SELECT * 
FROM mytable
LIMIT 5;
```
```
SELECT *
FROM mytable
LIMIT 0, 5;
```
返回第3～5行：
```
SELECT *
FROM mytable
LIMIT 2, 3;
```

### 八、排序
* ASC：升序（默认）
* DESC：降序
可以按多个列进行排序，并且为每个列指定不同的排序方式：
```
SELECT *
FROM mytable
ORDER BY col1 DESC, col2 ASC;
```

### 九、过滤
不进行过滤的数据非常大，导致通过网络传输了多余的数据，从而浪费了网络带宽。因此尽量使用 SQL 语句来过滤不必要的数据，而不是传输所有的数据到客户端中然后由客户端进行过滤。

```
SELECT * 
FROM mytable
WHERE col IS NULL;
```
下表显示了 WHERE 字句可用的操作符。
|操作符|说明|
|----|----|
| = | 等于 |
| < | 小于 |
| > | 大于 |
| <>!= | 不等于 |
| <=!> | 小于等于 |
| >=!= | 大于等于 |
| BETWEEN | 在两个值之间 |
| IS NULL | 为 NULL 值 |

应该注意到，NULL 和 0、空字符串都不同。  

AND 和 OR 用于连接多个过滤条件。优先处理 AND，当一个过滤表达式涉及到多个 AND 和 OR 时，可以使用（）来决定优先级，时的优先级关系更清晰。  

IN 操作符用于匹配一组值，其后也可以接一个 SELECT 子句，从而匹配子查询得到的一组值。  

NOT 操作符用于否定一个条件。

### 十、通配符
通配符也是用在过滤语句中，但它只能用于文本字段。  
* % 匹配 >=0 个任意字符；
* _ 匹配 ==1 个任意字符；
* [] 可以匹配集合内的字符，例如 [ab]将匹配字符 a 或者 b。用脱字符 ^ 可以对其进行否定，也就是不匹配集合内的字符。  
使用 LIKE 来进行通配符匹配。
```
SELECT *
FROM mytable
WHERE col LIKE '[^AB]%'; -- 不以 A 和 B 开头的任意文本
```
不要滥用通配符，通配符位于开头处匹配会非常慢。



















