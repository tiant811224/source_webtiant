---
title: "[做点有趣的]C++做一个哈夫曼压缩软件"
date: 2021-05-25T07:12:32+08:00
# draft: true
tags: [
    "C++"
]
categories: [
    "项目",
]
---

### 前言
这是以前的一个实训周作业，核心是哈夫曼编码和解码，软件界面由 QT 实现。代码量很小，比较简单。

源代码： [https://gitee.com/lzxqaq/zxhan.git](https://gitee.com/lzxqaq/zxhan.git)

介绍：[https://lzxqaq.com/post/zxhan/](https://lzxqaq.com/post/zxhan/)

算法参考：[Huffman压缩真正的C++实现](https://blog.csdn.net/small_hacker/article/details/52843738)


运行环境：Linux 系统（Windows系统下运行尚有 bug ),开发环境为 QT Creator。

运行截图：
<div  align="center">    
 <img src="https://cdn.jsdelivr.net/gh/lzxqaq/zxhan@master/images/zxhan.png" width = "500" height = "200" alt="图片名称" align=center /></div>
<!-- ![img](https://cdn.jsdelivr.net/gh/lzxqaq/zxhan@master/images/zxhan.png) -->


### 核心实现
```
void create_node_array();//构造包含字符及其频率的数组
void create_pq();//构造优先级队列
void create_huffman_tree();//构造哈夫曼树
void create_map_table(Node* node,bool);//根据哈夫曼树建立哈夫曼映射表
bool calculate_huffman_codes();//计算哈夫曼编码
bool do_compress();//开始压缩
bool rebuid_huffman_tree();//从哈夫曼编码文件中重构哈夫曼树
void decode_huffman();//根据重构的哈夫曼树解码文件

```

### 后续

该项目仍有许多不足之处，如果你对该项目有任何意见或建议，欢迎[联系我](https://lzxqaq.com/about/)。如有任何问题，亦可与我一同探讨。

