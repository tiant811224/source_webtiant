---
title: "Java I/O"
date: 2021-01-12T08:46:08+08:00
author: "罗泽勋"
categories: [
    "Java"
]
---

### 一、概览
Java 的 I/O 大概可以分成以下几类：
* 磁盘操作：File
* 字节操作：InputStream 和 OutputStream 
* 对象操作：Serializable
* 网络操作：Socket
* 新的输入/输出：NIO

### 二、磁盘操作
File 类可以用于表示文件和目录的信息，但是它不表示文件的内容。
递归地列出一个目录的所有文件：
```
public static void listAllFiles(File dir){
    if(dir == null || !dir.exists()){
        return ;
    }
    if(dir.isFile()) {
        System.out.println(dir.getName());
        return ;
    }
    for (File:file : dir.listFiles()) {
        listAllFiles(file);
    }
}
```
从 Java 7 开始，可以使用 Paths 和 Files 代替 File。

### 三、字节操作
#### 实现文件复制
```
public static void copuFile(String src, String dist) throws IOException {
    FileInputStream in = new FileInputStream(src);
    FileOutputStream out = new FileOutStream(dist);

    byte[] buffer = new byte[20 * 1024];
    int cnt;
    // read() 最多读 buffer.length 个字节
    // 返回的是实际读取的个数
    // 返回 -1 的时候表示读到 eof,即文件结尾
    while ((cnt = in.read(buffer, 0, buffer.length)) != -1) {
        out.write(buffer, 0, cnt);
    }

    in.close();
    out.close();
}
```

#### 装饰者模式
Java I/O 使用了装饰者模式来实现。

实例化一个具有缓存功能的字节流对象时，只需要在 FileInputStream 对象上再套一层 BufferedInputStream 对象即可。

### 七、NIO
新的输入/输出（NIO）库是在 JDK1.4中引入的，弥补了原来的 I/O 的不足，提供了高速的、面向块的 I/O。







































