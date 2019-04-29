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

