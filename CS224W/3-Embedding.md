# 图嵌入表示学习

> 同济子豪兄 2023-1-18
>

### 概述

避免了人工特征工程。

图嵌入向量：低维，连续，稠密，嵌入d维空间的一个点

deepwalk

### 基本框架

编码器：嵌入到某个空间，得到向量，使得图中相似的节点嵌入向量相似度也大，这也是优化目标

解码器：相似度，（人为定义相似度）可以是内积

最简单的编码器：查表(浅编码器)

![image-20230610152804768](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20230610152804768.png)

![image-20230610152916991](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20230610152916991.png)



![image-20230610152826911](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20230610152826911.png)

### 基于随机游走的嵌入方法

<img src="C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20230610153236222.png" alt="image-20230610153236222" style="zoom: 50%;" />

从图中每个结点u开始随机固定长度游走，以某个策略R

对每个节点u，记录从它开始的游走路径上经过的节点集合N_R(u)，可以重复

最优化嵌入：给定节点u,预测到N_R(u)的概率

![image-20230610155231400](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20230610155231400.png)

![image-20230610155330457](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20230610155330457.png)

复杂度O(|V^2|): 降低复杂度方法，负采样，即现在只要算和这些负样本的内积，而不是和所有人的内积。

### Node2vec

控制随机游走策略的一种方式

两个参数：

- p: 返回前一节点
- q: ratio of BFS vs. DFS

### 矩阵分解和随机游走

将邻接矩阵A分解，
$$
A = Z^TZ
$$
Z可以视为一个图嵌入。使用的相似度很简单，如果两个结点之间有一条边，就相似。

![image-20230610161259830](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20230610161259830.png)

如果结点2,4之间有边，那Z.T的第2行和Z的第四列相乘为1，否则为0（正交）。

Z矩阵行小于列，只能通过数值计算方法近似求解。

![image-20230610161547673](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20230610161547673.png)

deepwalk可以视为对以下矩阵做分解：

![image-20230610161625870](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20230610161625870.png)

### 讨论

缺点：

只能对静态数据

与起点相近的结点肯定会被采样

### 嵌入整张图

直接对所有节点嵌入求和

引入虚拟节点，与所有节点相连，求虚拟节点的嵌入

**匿名随机游走**

见到一个没见过的节点就发一个号，认号不认人

![image-20230610162256086](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20230610162256086.png)

将bag-of-anonymous walks作为嵌入

![image-20230610162604918](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20230610162604918.png)



## 视频

中文精讲视频：https://www.bilibili.com/video/BV1AP4y1r7Pz

Youtube视频：

https://www.youtube.com/watch?v=rMq21iY61SE&list=PLoROMvodv4rPLKxIpqhjhPgdQy7imNkDn&index=7

https://www.youtube.com/watch?v=Xv0wRy66Big&list=PLoROMvodv4rPLKxIpqhjhPgdQy7imNkDn&index=8

https://www.youtube.com/watch?v=eliMLfJeu7A&list=PLoROMvodv4rPLKxIpqhjhPgdQy7imNkDn&index=9

https://www.youtube.com/watch?v=r12qJZZVtqc&list=PLoROMvodv4rPLKxIpqhjhPgdQy7imNkDn&index=13

## 本讲介绍

斯坦福大学CS224W图机器学习公开课-同济子豪兄中文精讲

课程大纲、中文笔记课件、论文笔记、代码、思考题、扩展阅读、答疑群：https://github.com/TommyZihao/zihao_course/tree/main/CS224W

本讲是图表示学习综述，介绍了图嵌入（节点嵌入）表示学习的基本框架和编码器-解码器架构，将节点嵌入映射为低维、连续、稠密向量。向量空间的相似度反映了对应节点在原图上的相似度。在同一个随机游走序列中共同出现的节点，视为相似节点，从而构建类似Word2Vec的自监督学习场景。衍生出DeepWalk、Node2Vec等基于随机游走的图嵌入方法。

从数学上，随机游走方法和矩阵分解是等价的。

进而讨论嵌入整张图的方法，可以通过所有节点嵌入向量聚合、引入虚拟节点、匿名随机游走等方法实现。

## 思考题

机器学习中的“表示学习”是做什么的？为什么要做表示学习？

CS224W整门课程，都对哪些研究对象进行了嵌入编码的表示学习操作？

图嵌入有什么用？

图嵌入有哪几种技术方案？各有什么优劣？

如何理解图嵌入向量的“低维、连续、稠密”

如何衡量两个节点是否“相似”？

图嵌入中，Decoder为什么用两个向量的数量积？

如何理解图嵌入中的Shallow Encoder和Deep Encoder？有何区别？

随机游走序列包含了哪些信息？

图机器学习和自然语言处理存在怎样的对应关系？

简述DeepWalk算法原理

简述Node2Vec算法原理

除了DeepWalk和Node2Vec之外，还有哪些基于随机游走的图嵌入算法？

同济子豪兄论文精读视频中，DeepWalk和Node2Vec也留了不少思考题，去看看吧

你是否能想出更科学的随机游走策略？

基于随机游走的图嵌入方法，都可以被统一成什么样的数学形式？

重新思考：为什么要把图表示成矩阵的形式？

## 特别感谢

赵冰辰（爱丁堡大学）

