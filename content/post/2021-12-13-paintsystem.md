---
title: "Qt6 绘制基础"
date: 2021-12-13T21:28:59+08:00
author: "罗泽勋"
slug: "paintsystem" 
series: ["Qt 绘制系统"] 

---

### 绘制系统

Qt 的绘制系统可以使用相同的 API 在屏幕和打印设备上进行绘制，并且主要基于 QPainter 、 QPaintDevice 和 QPaintEngine 类。

QPainter 用于执行绘图操作， QPaintDevice 是二维空间的抽象，可以使用 进行绘制 QPainter ， QPaintEngine 提供了画家用来绘制不同类型设备的接口。 该 QPaintEngine 类是由在内部使用 了QPainter 和 的QPaintDevice ，除非他们创建自己的设备的种类从应用程序员隐藏。 

![paintsystem](https://cdn.jsdelivr.net/gh/lzxqaq/jsdelivr@master/image/2021-12-13/paintsystem.png)

这种方法的主要好处是所有绘制都遵循相同的绘制管道，从而可以轻松添加对新功能的支持并为不受支持的功能提供默认实现。 


### 一、绘图示例

通常在 QWidget, QPixmap, QPixture, QPrinter 上面绘图。

#### 示例1 直接绘制：
```
MainWindow::MainWindow(QWidget *parent)
    : QMainWindow(parent)
{
    QLabel *label = new QLabel(this);
    label->resize(100, 100);

    QPixmap pixmap(100, 100);
    pixmap.fill(Qt::gray);

    QPainter painter(&pixmap);
    painter.drawRect(10, 10, 80, 80);
    painter.drawText(20, 30, "Hello World");

    label->setPixmap(pixmap);

    QVBoxLayout *layout = new QVBoxLayout();
    layout->addWidget(label);
    this->setLayout(layout);
    this->resize(200, 200);
}

```
运行结果：

![demo1](https://cdn.jsdelivr.net/gh/lzxqaq/jsdelivr@master/image/2021-12-13/demo1.png)

#### 示例2 paintEvent(QPaintEvent *) 函数中绘制：

```
void MainWindow::paintEvent(QPaintEvent *)
{
    QPainter painter(this);
    painter.setPen(Qt::gray);
    painter.setBrush(Qt::green);
    painter.drawRect(10, 10, 50, 50);
}
```

运行结果：

![demo2](https://cdn.jsdelivr.net/gh/lzxqaq/jsdelivr@master/image/2021-12-13/demo2.png)

### 二、坐标系

坐标系由 QPainter 类控制。绘图设备的默认坐标系的原点位于左上角。 该 X 值增加向右和 Y 值向下增加。 默认单位在基于像素的设备上是一个像素，在打印机上是一个点（1/72 英寸）。注意：原点在 Widget 的左上角而不是正中心，并且每个 Widget 都有自己独立的坐标系。 

QPainter 的逻辑坐标到物理 QPaintDevice 坐标的映射由 QPainter 的转换矩阵、视口和“窗口”处理。 默认情况下，逻辑坐标系和物理坐标系是一致的。 QPainter 还支持坐标变换（例如旋转和缩放）。 

#### 渲染 逻辑表示

图形基元的大小（宽度和高度）始终与其数学模型相对应，忽略绘制它的笔的宽度： 

![Paint-Base-Draw-Methods](https://cdn.jsdelivr.net/gh/lzxqaq/jsdelivr@master/image/2021-12-13/coordsys.png)

#### Window-Viewport 转换

使用 Window-Viewport 转换，您可以使逻辑坐标系适合您的偏好。 该机制还可用于使绘图代码独立于绘图设备。 例如，您可以通过调用 ，使逻辑坐标从 (-50, -50) 扩展到 (50, 50)，以 (0, 0) 为中心 QPainter::setWindow () 函数 ：

```
QPainter painter(this);
painter.setWindow(QRect(-50, -50, 100, 100));
```

现在，逻辑坐标 (-50,-50) 对应于绘制设备的物理坐标 (0, 0)。 独立于绘画设备，您的绘画代码将始终在指定的逻辑坐标上运行。 


### 三、常用函数

下图来自《C++ GUI Programming with Qt 4》，列出了 QPainter 常用的画图方法。

![Paint-Base-Draw-Methods](https://cdn.jsdelivr.net/gh/lzxqaq/jsdelivr@master/image/2021-12-13/Paint-Base-Draw-Methods.png)

#### 线 - drawLine()


#### 多线段 - drawLines()


#### 折线 - drawPolyline()


#### 多边形 - drawPolygon()


#### 矩形 - drawRect()


#### 圆角矩形 - drawRoundRect() & drawRoundedRect()


#### 圆、椭圆 - drawEllipse()


#### 弧、弦、饼图 - drawArc()、drawChord()、drawPie()


#### 绘制 QPixmap - drawPixmap()


#### 绘制 QImage - drawImage()

