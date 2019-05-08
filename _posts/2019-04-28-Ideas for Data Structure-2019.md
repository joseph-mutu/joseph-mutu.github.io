---
layout:     post   				    # 使用的布局（不需要改）
title:      Ideas for Data Structure 				# 标题 
subtitle:   Data Structure          #副标题
date:       2019-04-28 				# 时间
author:     WYX 						# 作者
header-img: img/1.6.png 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - data structure

---



# **Saving James Bond - Hard Version**

- 将每一个鳄鱼的坐标看做一个节点，初始节点的坐标为 0,0
- 根据最大跳跃距离为条件，设置每一个节点之间是否连通（如果能跳过去，即为连通）
- dis[节点i] 表示当前节点到原点的最短距离
- 注意，在压入首节点的时候，要将距离从大到小排列再压入
- 使用无权最短路，利用队列，将一个顶点所有联通点进行一次更新



# 最小生成树

## Prim 算法

在 prim 算法中，以任意节点为起始点，构建一个 cost[1~nodes] 数组，代表当前节点距离当前生成树的最小距离

- 在 cost 数组中，每当添加一个节点进入生成树之后，要对 cost 数组进行更新 ,因为 cost[i] 代表当前节点距离生成树的距离。随着生成树节点的增多，cost也不断变化
  - 以 v5 举例
    - 当生成树包含 v1时， cost[v5] = 999
    - 生成树: v1, v2, cost[v5] = 10
    - 生成树: v1,v2,v4, cost[v5] = 7
    - 生成树:v1,v2,v4,v7, cost[v5] = 6
  - cost[i] 的变化范围就是节点的出度
- 每次选择距离生成树最近的一个节点，将其添加进生成树中。
- 如果没有最小节点可以选择，但是仍旧有节点遗留，说明树不连通

![](https://ae01.alicdn.com/kf/HTB1fHDwT8LoK1RjSZFu760n0XXaB.png)

## Kruskal 算法

在 Kruskal 算法中，以边为结点，每次选取权重最小但是不能构成回路的边

- 权重最小
- 无法构成回路
- 当边的数目到达 |V-1|(V 是结点的数目)时表示最小生成树完成，跳出

在 Kruskal 分别使用

- 最小堆 --> 挑选权重最小的边

- 并查集 --> 检查回路

  - 在检查回路的时候，是对相应边的两个结点进行合并以及检查
  - 使用路径压缩，使得一棵生成树只有一个父节点
  - 如果并查集中出现两个父节点，说明图不连通

  

# 排序

## 冒泡排序

具体算法如下：

- 开始指针指向初始数组0以及1位置，如果 data[0] > data[1],交换位置
- 指针移动至 1,2
- 重复交换
- 两重循环

特征

- 最坏的情况为 O($N^2$)

- 每一次遍历最大的值一定会在最底部

  



## 插入排序

给定一段数据，从下标为 1 的数据开始进行比对

假设给定数据序列为


$$
X = \lbrace3,2,1 \rbrace
$$


假设 X[i] 为手中拿到的牌，X[i-1~0]为已经有的牌，则当前比对的数据序列为


$$
X = \lbrace3,2 \rbrace
$$


记住当前的牌，如果 X[i-1] 大于 X[i], 则将 X[i-1] 向后挪一位，数据序列变为


$$
X = \lbrace3,3 \rbrace
$$


如果比对下标为0，或者不满足X[i-1] 大于 X[i], 寻找到位置，将手中的牌插入。在上述操作当中，将X[0] 挪位之后，i = 0 跳出

```python
def insert_sort(number,data):
	for p in range(1,number):
		tem = data[p]
		for i in range(p,-1,-1):
			if data[i-1] > tem and i > 0:
				data[i] = data[i-1]
			else:
				break
		data[i] = tem
	return data
```





## 希尔排序

希尔排序会使用插入排序

其核心在于定义增量序列，每次使用插入排序对增量序列进行排序

![](https://ae01.alicdn.com/kf/HTB1ETkCUSzqK1RjSZPcq6zTepXaf.jpg)

## 归并排序

## 快速排序 

## 表排序

## 基数排序





