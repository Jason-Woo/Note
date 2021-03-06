# CNN笔记

![1540353191597a9a101dbd3](.\img_cnn\1540353191597a9a101dbd3.gif)



![154035248788599038174f5](.\img_cnn\154035248788599038174f5.gif)

![154035248791818f75510d4](.\img_cnn\154035248791818f75510d4.gif)

为什么使用CNN而不是全连接？

* 展开图像丢失空间信息
* 参数过多效率低下
* 参数过多，网络过拟合

## 卷积层

![2019-06-19-juanji](.\img_cnn\2019-06-19-juanji.gif)

![1](.\img_cnn\1.PNG)

### 作用

核心层，产生了网络中大部分计算量（不是数据量）

* 滤波器/卷积 卷积层的参数是由一些科学系的滤波器集合构成的，每个滤波器宽度高度较小，但是深度和输入数据一致。
* 可以看作神经元的一个输出， 神经元只观察输入数据的一小部分，且和相邻神经元共享参数。
* 降低参数数量，由于卷积“权值共享”的特性，可以降低参数数量。



### 理解感受野

每个神经元只与输入的一个局部区域连接。连接在宽高上是局部的，但在深度上与输入一致。

![0](.\img_cnn\0.PNG)

上图中红色为输入[32x32x3]，如果感受野（滤波器尺寸）为5x5，那么卷积层中每个神经元有[5x5x3]个权重。每一层与自己的卷积模板做完卷积后加起来，再加上偏置（一个卷积核只有一个偏置）



### 神经元的空间排列

三个超参数

* 深度 depth 和使用的滤波器（卷积和）数量一致
* 步长 stride 滤波器每次移动的像素
* 零填充 zero-padding 用来保持输入数据体在空间上的尺寸，使得输入和输出的宽高都相等



### 权值共享

权值共享是基于这样的一个合理的假设：如果一个特征在计算某个空间位置 (x1,y1)(x1,y1) 的时候有用，那么它在计算另一个不同位置 (x2,y2)(x2,y2) 的时候也有用。

基于这个假设，可以显著地减少参数数量。



### 卷积层超参数

![2](.\img_cnn\2.PNG)

![3](.\img_cnn\3.PNG)

![0](.\img_cnn\4.PNG)

![0](.\img_cnn\5.PNG)



## 池化层

作用是逐渐降低数据体的空间尺寸，这样的话就能减少网络中参数的数量，使得计算资源耗费变少，也能有效控制过拟合。

![2019-06-19-chihua](.\img_cnn\2019-06-19-chihua.gif)

![6](.\img_cnn\6.PNG)

上图中$W_1=H_1=20, D_1=1,F=10,S=10$

历史上曾用平均池化和L-2范式池化，但目前最大池化效果最好



## 全连接

这个常规神经网络中一样，它们的激活可以先用矩阵乘法，再加上偏差。

![7](.\img_cnn\7.PNG)

![8](.\img_cnn\8.PNG)



## 神经网络的结构

一般包括：卷积层，池化层，全连接层

### 层的排列规律

![8](.\img_cnn\9.PNG)

### 卷积层的大小选择

几个小滤波器卷积层的组合比一个大滤波器卷积层好：

* 个卷积层与非线性的激活层交替的结构，比单一卷积层的结构更能提取出深层的更好的特征
* 使用的参数更少
* 但是反向传播占用内存



### 层的尺寸设置规律

- **输入层** ，应该能被2整除很多次。常用数字包括32，64，96或224，384和512。
- **卷积层** ，应该使用小尺寸滤波器（比如3x3或最多5x5），使用步长S=1。还有一点非常重要，就是对输入数据进行零填充，这样卷积层就不会改变输入数据在空间维度上的尺寸。比如，当感受野尺寸F=3，那就使用P=1来保持输入尺寸。当F=5,P=2，一般对于任意F，当P=(F-1)/2的时候能保持输入尺寸。如果必须使用更大的滤波器尺寸（比如7x7之类），通常只用在第一个面对原始图像的卷积层上。
- **汇聚层** ，负责对输入数据的空间维度进行降采样。最常用的设置是用用2x2感受野（即F=2）的最大值汇聚，步长为2（S=2）。注意这一操作将会把输入数据中75%的激活数据丢弃（因为对宽度和高度都进行了2的降采样）。另一个不那么常用的设置是使用3x3的感受野，步长为2。最大值汇聚的感受野尺寸很少有超过3的，因为汇聚操作过于激烈，易造成数据信息丢失，这通常会导致算法性能变差。

**为何使用零填充**？使用零填充除了前面提到的可以让卷积层的输出数据保持和输入数据在空间维度的不变，还可以提高算法性能。如果卷积层值进行卷积而不进行零填充，那么数据体的尺寸就会略微减小，那么图像边缘的信息就会过快地损失掉。