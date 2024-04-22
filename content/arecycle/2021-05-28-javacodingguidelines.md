---
title: "【规约】 Java 编程篇"
date: 2021-05-28T10:36:40+08:00
tags: [
    "Java"
]
tags: [
    "编程规范"
]
draft: true

---
### 前言

“码出高效，码出质量”。本文整理了比较常见的 Java 编程规范，主要来源于《阿里巴巴 Java 开发手册》。

### （一）命名风格
1.【强制】类名使用 UpperCamelCase风格，但以下情形例外：DO  /  BO  /  DTO  /  VO  /  AO  /  PO  /  UID等。
    正例：MarcoPolo  /  UserDO  /  XmlService  /  TcpUdpDeal  /TaPromotion

2.【强制】方法名、参数名、成员变量、局部变量都统一使用 lowerCamelCase 风格，必须遵从驼峰形式。  
    正例：localValue  /  getHttpMessage()  /  inputUserId

3.【强制】常量命名全部大写，单词间用下划线隔开，力求语义表达完整清楚，不要嫌名字长。  
    正例：MAX_STOCK_COUNT

4.【强制】抽象类命名使用 Abstract 或 Base 开头；异常类命名使用 Exception 结尾；测试类命名以它要测试的类的名称开始，以 Test 结尾。

5.【强制】类型与中括号紧挨起来表示数组。  
    正例：int[] arrayDemo;

6.【强制】POJO 类中布尔类型的变量，都不要加 is 前缀。

7.【强制】包名统一使用小写，点分隔符之间有且只有一个自然语义的英语单词。包名统一使用单数形式，但是类名如果有复数含义，类名可以使用复数形式。  
    正例：应用工具类包名为 com.alibaba.ai.util、类名为 MessageUtils。

8.【强制】杜绝完全不规范的缩写，任何自定义编程元素在命名时，使用尽量完整的单词组合来表达其意。

9.【推荐】如果模块、接口、类、方法使用了设计模式，在命名时需体系那具体模式。

10.【推荐】接口类的方法和属性不要加任何修饰符号（public 也不要加），保持代码整洁，并加上有效的 Javadoc 注释。

11.【强制】对于 Service 和 DAO 类，基于 SOA 的理念，暴露出来的一定时接口，内部的实现类用 Impl 的后缀与接口区别。  
    正例：CacheServiceImpl 实现 CacheService。

12.【参考】各层命名规约：
    A）Service/DAO 层方法命名规约  
        1）获取单个对象的方法用 get 做前缀。  
        2）获取多个对象的方法用 list 做前缀，复数形式结尾如： listObjects。  
        3）获取统计值的方法用 count 做前缀。  
        4）插入的方法用 save/insert 做前缀  
        5）删除的方法用 remove/delete 做前缀  
        6）修改的方法用 update 做前缀  
    B）领域模型命名规约  
        1）数据对象：xxxDO，xxx 即为数据表名。  
        2）数据传输对象：xxxDTO，xxx 为业务领域相关的名称。  
        3）展示对象：xxxVO，xxx 一般为网页名称。  
        4) POJO 是 DO/DTO/BO/VO 的统称，禁止命名为 xxxPOJO。

13.【强制】不允许任何魔法值（即未经预先i当以的常量）出现在代码中。  
    反例：String key = "ID#taobao_" + tradeId;

14.【强制】代码格式，具体见下面正例。
    正例：
```
    public static void main(String[] args) {
        // 缩进4个空格，注释内空1格
        String say = "hello";
        // 运算符左右必须有一个空格
        int flag = 0;
        // 关键词 if 与括号之间必须有一个空格
        if (flag == 0) {
            System.out.println(say);
        }

        // 左大括号前加空格且不换行，左大括号后换行。
        if (flag == 0) {
            System.out.println("world");
        //  右大括号前换行，右大括号后有 else，不用换行、
        }
        else {
            System.out.printLn("ok");
        }