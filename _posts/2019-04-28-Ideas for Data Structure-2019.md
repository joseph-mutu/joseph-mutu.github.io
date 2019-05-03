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