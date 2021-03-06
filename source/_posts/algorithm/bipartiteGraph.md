---
title: 二分图
---
参考连接:
https://baike.baidu.com/item/%E4%BA%8C%E5%88%86%E5%9B%BE/9089095?fr=aladdin
https://blog.csdn.net/sixdaycoder/article/details/47720471
http://blog.jobbole.com/106084/
<!--more-->

**几个概念**

1. 二分图
二分图又称作二部图，是图论中的一种特殊模型。 设G=(V,E)是一个无向图，如果顶点V可分割为两个互不相交的子集(A,B)，并且图中的每条边（i，j）所关联的两个顶点i和j分别属于这两个不同的顶点集(i in A,j in B)，则称图G为一个二分图。

2. 最大匹配问题
给定一个二分图G，在G的一个子图M中，M的边集中的任意两条边都不依附于同一个顶点，则称M是一个匹配.选择这样的边数最大的子集称为图的最大匹配问题。

3. 完备匹配(complete matching)是匹配了二分图较小集合（二分图X，Y中小的那个）的所有点的匹配。
   完美匹配(perfect matching)是匹配了所有点的匹配。

4. 最佳匹配
如果二分图的每条边都有一个权（可以是负数），要求一种完备匹配方案，使得所有匹配边的权和最大，记做最佳完美匹配。（特殊的，当所有边的权为1时，就是最大完备匹配问题）

5. 最小顶点覆盖
在二分图中寻找一个尽量小的点集，使图中每一条边至少有一个点在该点集中。
　　最小顶点覆盖 == 最大匹配。

6. 最小路径覆盖
在二分图中寻找一个尽量小的边集，使图中每一个点都是该边集中某条边的端点。
　　最小路径覆盖 == |V| - 最大匹配。

7. 最大独立集
在N个点中选出来一个最大点集，使这个点集中的任意两点之间都没有边。
　　最大独立集 == 顶点数 - 最大匹配。

8. 交替路和增广路
![](http://ww2.sinaimg.cn/large/7cc829d3gw1f89lnzbetkj204j04u74f.jpg)

交替路：从一个未匹配点出发，依次经过非匹配边、匹配边、非匹配边…形成的路径叫交替路。

![](http://ww2.sinaimg.cn/mw690/7cc829d3gw1f89lo04o2wj207y01y3yi.jpg)

增广路的定义(也称增广轨或交错轨):从一个未匹配点出发，走交替路，如果途径另一个未匹配点（出发的点不算），则这条交替路称为增广路.

由增广路的定义可以推出下述三个结论:
1) P的路径长度必定为奇数，第一条边和最后一条边都不属于M.
2) P经过取反操作可以得到一个更大的匹配M'.
3) M为G的最大匹配当且仅当不存在相对于M的增广路径.

**匈牙利算法和km算法**
https://www.cnblogs.com/logosG/p/logos.html
https://blog.csdn.net/ling_wang/article/details/79830980?utm_source=blogxgwz3
https://www.cnblogs.com/zhanzhao/p/3895880.html
https://blog.csdn.net/ling_wang/article/details/79830980?utm_source=blogxgwz3
