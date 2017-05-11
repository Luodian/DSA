# 最小生成树（MST）

## 一、背景（Back ground）

**最小生成树**是一副连通加权无向图中一棵权值最小的生成树。

用稍微数学一点的语言描述是这样的：

在一个给定的无向图 $G = (V,E)$ 中，$(U,V)$代表连接顶点$ u $与顶点 $v$ 的边，$w$ 代表此边上的权重。若存在 $T$ 为 $E$ 的子集，且 $T$ 为无循环图，使得 $w(T) = \Sigma^{}_{(u,v) \in T} w(u,v)$ 中的 $T$ 最小，则称 $T$ 为 $G$ 的最小权重生成树，简称最小生成树。

在现实生活中，最小生成树的应用主要在于给定一个图集 $G$ 表示，我们去求出最小花费边集 $T$ ，通过连接这个边集 $T$ 使得连通的。



## 二、几种算法

### Prim

#### 基础思想（Basic Concept）：

它是从点的方面考虑构建一颗MST，设图 $G$ 顶点集合为 $U$，首先任意选择图 $G$ 中的一点作为起始点 $a$ ，将该点加入集合 $V$，再从集合$U - V$中找到另一点 $b$ 使得点 $b$ 到 $V$ 中任意一点的权值最小，此时将 $b$ 点也加入集合 $V$；以此类推，现在的集合$ V=\{a，b\}$，再从集合 $U-V$ 中找到另一点 $c$ 使得点 $c$ 到 $V$ 中任意一点的权值最小，此时将 $c$ 点加入集合 $V$ ，直至所有顶点全部被加入 $V$ ，此时就构建出了一颗MST。因为有N个顶点，所以该MST就有 $N-1$ 条边，每一次向集合 $V$ 中加入一个点，就意味着找到一条 MST 的边。

![pic](http://i4.buimg.com/567571/b0105dd6307a3db6.gif)

其实真的，十分十分的像 Djikstra 最短路算法对吧！

> Djikstra 是根据 dist 数组去贪心，Prim算法则是根据最小生成树点集 $V$ 领接的最小边去进行贪心。

#### 时空复杂度（Complexity）

- Prim算法的时间复杂度分以下几种实现方式：

  1. 邻接矩阵：$O(V^{2})$
  2. 领接表 + 二叉堆：$O((V+E) * \log_2{V})$
  3. 领接表 + 斐波拉契堆：$O(E + V * \log_2{V})$

- 空间复杂度

  显然无论怎么做，它的空间复杂度都只跟存储方式有关，而在二叉堆和斐波拉契堆优化时所用的辅助存储空间都是和存储空间同阶的函数。

#### 实现（Implemention）：

见代码



### Kruskal 算法

#### 基本思想

Kruskal 算法是一种标准的贪心方法，新建图G，G中拥有原图中相同的节点，但没有边，将原图中所有的边按权值从小到大排序，从权值最小的边开始，如果这条边连接的两个节点于图G中不在同一个连通分量中，则添加这条边到图G中，重复3，直至图G中所有的节点都在同一个连通分量中。

#### 时空复杂度（Complexity）

- Kruskal 算法的时间复杂度分以下几种实现方式：

  1. 领接表 + 排序：$O(E * \log_2{E})$
  2. 领接表 + 二叉堆（用于维护前 K 条最小边）：$O(N + E * \log_2{N})$

- 空间复杂度

  根据以上的实现方式不同，空间复杂度也不尽相同，其中第二种实现方式需要 $O(N)$ 的堆辅助空间。

  ​


#### 实现（Implemention）：

见代码

## 三、测试（Test）

对于本次实验我给定了分别是小，中，稠密图三个数据集。

1. 对于小数据集

   ```cpp
   Input：

   6 10  //点数，边数
   1 3 1 //U，V，W
   1 2 6
   1 4 5
   2 5 3
   3 5 6
   3 6 4
   5 6 6
   3 4 5
   3 2 5
   6 4 2
   Output：

   The minimal cost is : 15
   The minimal spanning tree contains edge as below : 
   Output as form of tuple (u,v,w)
   1 3 1
   3 6 4
   6 4 2
   3 2 5
   2 5 3
   ```

   使用Networkx作图如下，其中将MST树边标记为了更醒目的颜色。

   ![pic](http://i4.buimg.com/567571/8358107b1a10c832.png)

2. 对于稍微较大的数据集（50个点，184条边）

   ```cpp
   Input:
   50 184
   18 2 215
   4 23 392
   42 19 163
   15 24 365
   27 45 978
   36 23 928
   3 35 310
   ……
     
   output:
   The minimal cost is : 7570
   The time cost is : 3.9e-05s. 
   ```

   数据可视化结果如下：

   ![](http://i1.piimg.com/567571/b41003952d017256.png)

3. 稠密图数据集（1000个点，392582条边）

   ```cpp
   Input:
   1000 392582
   285 761 141
   300 944 976
   607 201 404
   952 630 568
   293 256 889
   539 404 546
   227 492 437
   570 449 831
   192 560 777
   875 622 737
   ……
   ```

   对于稠密图（点少，边多）的情况，我们对 Prim 算法以及 Kruskal 算法进行了比较测试。

   ```cpp
   Output:
   When the graph is a real dense graph
   (1000 nodes, 392582 edges.)
   If we still using prim to solve it ? 
   The minimal cost is : 1982
   The time cost is : 0.077174s. 

   If we use kruskal to solve it ? 
   The minimal cost is : 1982
   The time cost is : 0.055812s. 
   ```

   可以看见，在对于稠密图操作中，从理论分析上应该是Prim算法具有更高的效率，但是在实际的操作中我们却发现是Kruskal算法更胜一筹，对于这个问题我进行了一些思考，也请教了学长和老师。

   对于上述两个算法的分析，我们都其实给出的是最坏时间复杂度的分析，其中Kruskal算法虽然是$O(E * \log_2{E})$，但是在实际的操作中如果给出的图连通性很好，我们是可以发现其可以在遍历前$N - 1$条边的情况下基本就可以找到近似解，因此其实际上耗费时间的步骤只产生在给边排序的过程中，因此实际上的Kruskal 算法在平均复杂度的情况下并不会比Prim慢很多。


## 四、总结

1. Kruskal编程实现更容易，其中有维护边集和排序两种方式，但是时间与空间通常是不能够兼得的，这也很好的体现了算法设计中Trade off的思想。
2. 待续