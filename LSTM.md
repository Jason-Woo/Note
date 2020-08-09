# 理解LSTM

## RNN

###  RNN的结构缺陷

RNN结构共享一组(U,W,b)，容易迭代训练时梯度消失或梯度爆炸



<img src="\img_lstm\0.PNG" alt="0" style="zoom:60%;" />

### 长期依赖Long-Term Dependencies问题

<img src="\img_lstm\4.PNG" alt="0" style="zoom:60%;" />

在这个间隔不断增大时，RNN 会丧失学习到连接如此远的信息的能力。

<img src="\img_lstm\5.PNG" alt="0" style="zoom:60%;" />

在标准的RNN中重复的模块结构非常简单，只有一个tanh$[(-\infty,\infty)\rightarrow(-1,1)]$层

## LSTM

长短期记忆（Long short-term memory, LSTM）是一种特殊的RNN，主要是为了解决长序列训练过程中的梯度消失和梯度爆炸问题。简单来说，就是相比普通的RNN，LSTM能够在更长的序列中有更好的表现。

<img src="\img_lstm\6.PNG" alt="0" style="zoom:60%;" />

LSTM结构（图右）和普通RNN的主要输入输出区别如下所示。

<img src="\img_lstm\1.jpg" alt="1" style="zoom:80%;" />

LSTM相比于RNN有两个传输状态，cell state和hidden state。RNN中的h对应LSTM中的c。

对于传递下去的c改变的很慢，h则在不同节点下有很大区别。



### LSTM结构

<img src="\img_lstm\2.png" alt="1" style="zoom:80%;" />

四个状态在LSTM中的使用

<img src="\img_lstm\3.png" alt="1" style="zoom:80%;" />



LSTM内部主要有三个阶段

* 忘记阶段，对上一个节点传进来的输入进行选择性忘记，通过$z^f$作为门控，控制$c^{t-1}$哪一些要留哪些要忘

* 选择记忆，主要是对输入$x^t$进行选择记忆，当前输入内容由$z$表示，$z^i$作为门控。

  * 上面两步结果相加即得到传给下一个状态的$c^t$

* 输出阶段，决定哪些会当成输出，$z^o$作为门控。并且对上一阶段得到的$c^t$进行了放缩

  * 与普通RNN类似，输出$y^t$由$h^t$变化得到

  
  
  ## 理解LSTM网络
  
  <img src="\img_lstm\6.PNG" alt="0" style="zoom:60%;" />
  
  <img src="\img_lstm\7.PNG" alt="0" style="zoom:60%;" />

### LSTM的核心思想

LSTM 的关键就是细胞状态，水平线在图上方贯穿运行。

细胞状态类似于传送带。直接在整个链上运行，只有一些少量的线性交互。信息在上面流传保持不变会很容易。

<img src="\img_lstm\8.PNG" alt="0" style="zoom:60%;" />

LSTM 有通过精心设计的称作为“门”的结构来去除或者增加信息到细胞状态的能力。门是一种让信息选择式通过的方法。他们包含一个 `sigmoid` 神经网络层和一个按位的乘法操作。

<img src="\img_lstm\9.PNG" alt="0" style="zoom:60%;" />

<img src="\img_lstm\10.PNG" alt="0" style="zoom:60%;" />

<img src="\img_lstm\11.PNG" alt="0" style="zoom:60%;" />

<img src="\img_lstm\12.PNG" alt="0" style="zoom:60%;" />

<img src="\img_lstm\13.PNG" alt="0" style="zoom:60%;" />