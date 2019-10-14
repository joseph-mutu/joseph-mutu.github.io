---
layout:     post   				    # 使用的布局（不需要改）
title:      Machine Leanring Models Summary 				# 标题 
subtitle:   machine learning           #副标题
date:       2019-10-02 				# 时间
author:     WYX 						# 作者
header-img: img/wuxia4.png 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - decision trees, Bayesian Inference, Navie Bayes

---

---

本文对常见的机器学习模型进行简单总结

---

## 线性回归

<center>
    <img src = "https://github.com/joseph-mutu/Pics/raw/master/%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0/%E7%BA%BF%E6%80%A7%E5%9B%9E%E5%BD%92.jpg" width = "200"/>
</center>

### 目标

寻找一组参数 $\textbf{w}$ 使得 $w^Tx$ 的值与其标签 $\textbf{y}$ 距离达到最小

#### 目标函数

目标函数根据最小二乘估计定义如下：
$$
L(w) = \sum _{i=1}^N ||w^Tx_i - y_i||^2 \\
= \sum _{i=1}^N \sqrt{(w^Tx_i - y_i)^2} ^2 \\
= \sum _{i=1}^N (w^Tx_i - y_i)^2 \tag{1}
$$
现设数据 $x_i$ 存在 K 个维度，则 N 个样本 $x_i$ 构成一个矩阵，相应的，参数 $\textbf{w}$  以及数据的 `label` 也构成一个列向量


$$
\left(
\begin{matrix}
w_1 \\
w_2\\
w_3 \\
\vdots\\
w_K
\end{matrix}
\right) _{K \times 1}
~~~~~~~~~
\left(
\begin{matrix}
x_{11} & x_{12} & x_{13} & \ldots & x_{1K} \\
x_{21} & x_{22} & x_{23} & \ldots & x_{2K} \\
 & &\vdots \\
 x_{N1} & x_{N2} & x_{N3} & \ldots & x_{NK} \\
\end{matrix}
\right) _{N\times K}
~~~~~~~~~
\left(
\begin{matrix}
y_1 \\
y_2\\
y_3 \\
\vdots\\
y_N
\end{matrix}
\right) _{N \times 1}
$$



则根据上述表示，可以将 (1) 式重写为矩阵乘积的形式
$$
L(w) = (W^TX^T - Y^T)_{1\times N}(XW - Y)_{N\times 1} \\
= W^TX^TXW_{1\times 1} - W^TX^TY_{1\times 1} - Y^TXW_{1\times 1} + Y^TT_{1\times 1} \\
= W^TX^TXW_{1\times 1} - 2 W^TX^TY_{1\times 1} + Y^TT_{1\times 1}
\tag{2}
$$
为了求得最佳的参数 $w$ ，对 (2) 进行求导得
$$
\frac{\partial L(W)}{\partial W} = 2X^TXW - 2X^TY \tag{3}
$$
令 （3）为 0，则可得
$$
2X^TXW - 2X^TY = 0 \\
\Rightarrow X^TXW = X^TY \\
\Rightarrow (X^TX)^{-1}X^TXW = (X^TX)^{-1}X^TY \\
\Rightarrow W = \underbrace{(X^TX)^{-1}}_{\bf{X^{\dagger}}}X^TY
$$
其中 $X^{\dagger}$ 称为伪逆矩阵

#### 半正定矩阵与正定矩阵

假设矩阵 $\textbf{A}$  ，对于任意非零向量 $\textbf{x}$ 存在,
$$
\textbf{x}^T\textbf{A}\textbf{x} \geq 0
$$
则称矩阵 $\textbf{A}$ 为**半正定矩阵**

----

若矩阵 $\textbf{A}$，对于任意非零向量 $\textbf{x}$ 存在,


$$
\textbf{x}^T\textbf{A}\textbf{x} > 0
$$
则称矩阵 $\textbf{A}$ 为**正定矩阵**

##### 性质

- 正定矩阵的特征值均大于 0
- 正定矩阵的顺序主子式均为正
- 正定矩阵的行列式均为正

#### 矩阵可逆的条件

- 正定矩阵
- 特征值均大于 0 
- 顺序主子式均为正



#### 概率角度

<center>
    <img src = "https://github.com/joseph-mutu/Pics/raw/master/%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0/%E7%BA%BF%E6%80%A7%E5%9B%9E%E5%BD%92--%E6%A6%82%E7%8E%87%E8%A7%92%E5%BA%A6.jpg" width = "700"/>
</center>



在给定一组 $\textbf{w}$ 的初始值以及 $\textbf{x} $ 的情况下，其预测值 $\hat{\textbf{y}}$  可以唯一确定，则数据的真实标签可以假定与噪声 $\epsilon$ 具有以下关系。
$$
y = w^Tx + \epsilon
$$
假设噪声 $\epsilon$ 服从均值为 0 ，方差为 $\delta^2$ 的高斯分布，即 $\epsilon \sim N(0,\delta^2)$, 则
$$
y \sim N(w^Tx,\delta^2)
$$
这里的解释参照上图。

如果假设参数$\textbf{w}$ 同样存在一个先验，即
$$
w \sim N(0,\delta_0 ^2) 
$$
则可以推得与 L2 正则化相同的结果

**正则化与先验具有相同的性质**

### 线性分类

#### 感知机

##### 模型

如下图所示，感知机意在找到一个超平面，将数据集分开，其中$\bf{w}$ 为超平面的法向量，其方向与超平面垂直，因为 $\bf{w}$ 的模不改变其方向，所以限定 $|\bf{w}| = 1$，则一个样本点是否为证，由该样本点$x$与 $\bf{w}$ 的夹角决定，即
$$
w \cdot x = |w||x|\cdot cos\theta \\
\theta < 90, cos \theta > 0 \\
\theta > 90, cos \theta < 0
$$

<center> 
    <img src = "https://github.com/joseph-mutu/Pics/raw/master/%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0/perception.jpg"width = "300"/><img src = "https://github.com/joseph-mutu/Pics/raw/master/%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0/perception2.jpg"width = "300">
</center>

##### Loss Function

最自然的选择为错误分类样本点的个数，但是因为该函数不可微也不可导，所以将**错误样本**距离超平面的距离作为损失函数，即
$$
\mathcal{L} = \frac{w\cdot x + b}{||w||}
$$
其中
$$
||w|| = \sqrt{\sum _{i=1}^K w_i^2},假设~ w ~ 为 K 维向量
$$

###### 函数间隔

在二分类的情况下，对于误分类的样本，
$$
y_i(w\cdot x +b) < 0 \tag{1}
$$
假设 $y_i = 1$ 表示该样本实际为正类，但是 $w^Tx + b < 0$， 使得该样本分类错误，

假设 $y_i = -1$ 表示该样本实际为负类，但是 $w^Tx + b > 0$,使得该样本分类到正类，

以上两种情况均满足  (1)

则损失函数可以表示为
$$
\mathcal{L(w,b)} = \arg \max \sum_{i\in\lbrace 错误样本点 \rbrace} y_i(w\cdot x_i + b) \\
= \arg \min -\sum_{i\in\lbrace 错误样本点 \rbrace} y_i(w\cdot x_i + b) 
$$

###### SGD

$$
\frac{\partial \mathcal{L}}{\partial \bf{w}} = - y_i\underbrace{\bf{w}\cdot x_i }_{将w与 b合并} \\
= - y_ix_i
$$



## 决策树

只要在数据的属性不完全重合的情况下，决策树总能在训练数据上达到 `100%` 的准确度，在此情况下，最后的叶节点多数只有一个数据，所以存在`overfitting`的问题

对于决策树模型，有以下几个问题：

- 如何选取最优的属性进行当前节点的划分
  - 信息增益（Information Gain） $\rightarrow ID3$
  - 信息增益率（Information Gain Ratio） $\rightarrow C4.5$
  - 基尼系数 （Gini）$\rightarrow CART$
- 在节点中数据类别不统一的时候
  - 是继续选择属性划分
    - 有过拟合的风险
  - 还是将当前节点置为叶节点
    - 何时停止属性的划分
- 如何剪枝
  - 预剪枝 (Pre-pruning)
  - 后剪枝 (Post-pruning)

决策树的经典模型分为以下三种：

- 使用信息增益的模型
- C4.5, 使用信息增益率进行当前最优属性的选取
- CART Tree

### 选取最优属性

#### 熵

当前节点的 Entropy 的计算公式如下:
$$
Ent(D) = -\sum _{i=1}^K p_ilog_2p_k
$$
熵衡量了当前节点的 `不确定性` 的大小

其中 $p_i$ 为某一类别（如果是二分类，则就是正负类别的数据）占当前节点中**整体数据**数目的比例，K 为当前属性一共具有 K 种可能值

#### 信息增益

下式中，D 为当前节点的数据， $a$ 为当前选取的属性，$|D^v|$ 表示在 a 特征的第 $v$ 个分量上具有的数据数目 


$$
Gain(D,a) = Ent(D) - \sum_{v=1} ^V \frac{|D^v|}{|D|}Ent(D^v) 
$$


信息增益表示，用属性 $a$ 对当前数据 D 进行划分，获得的增益有多大。因为属性 $a$ 可以有 V 个不同的取值，所以对其进行加权加和

###### 问题

因为熵衡量不确定性的大小，假设当前节点只存在一个数据，则其熵为 $$  $$ 
$$
1\times log_2 1 = 0
$$
所以当数据越纯，熵的增益就越大。**信息增益倾向于取值数目较多的属性**。

#### 信息增益率

信息增益率就上述问题进行了限制，计算公式如下：
$$
Gain_ratio(D,a) = \frac{Gain(D,a)}{IV(a)}, \ IV(a) = -\sum_{v=1}^V \frac{|D^v|}{|D|}log_2\frac{|D^v|}{|D|} 
$$


 假设 a 有 2 种取值，一共两个数据，则其 $IV(a)$ 为
$$
IV(a) = -2\frac{1}{2}log_2\frac{1}{2}= 1
$$
假设 a 有 3 种取值，一共 3 个数据，则其 $IV(a)$ 为
$$
IV(a) = -3\frac{1}{3}log_2\frac{1}{3}= log_23 > 1
$$
所以，一般若属性 $a$ 的取值数目（V）越多，则其 $IV(a)$ 也会越大

#### 基尼系数

假设样本存在 K 个类别，其中属于第 k 类的概率为 $p_k$ ,则基尼系数为
$$
Gini(D) = \sum_{k=1}^{K} p_k(1-p_k) = 1 - \sum_{k=1}^{K} p_k^2
$$
基尼系数表示：随机抽取两个样本，其不一样的概率。则基尼系数越小，表示样本越纯。则选择属性能使其达到最小基尼指数。根据基尼系数选取最优属性为： 
$$
Gini(D,a) =\sum_{v=1}^V \frac{|D^v|}{|D|}Gini(D^v)
$$


### 剪枝

剪枝是为了降低决策树过拟合的风险，分为两种。剪枝的实现需要用到 `Validation set`

#### 预剪枝 pre-pruning

- 在 `training set`上选取最优的属性之后测得一个准确度
  - 比如选取属性 a 对根节点进行划分，一共分为 $V$ 个子集，在所有的子集上数分类错误的数据，然后算出分类错误的数据占**整体数据**的比例
  - 将 `validation set` 数据同样应用到该树上，以同样的方法测得一个准确度
  - 比较两者的准确度，如果 `validation set` 上准确度较 `training set` 有提升，则保留该属性，否则拒绝该属性划分阿萨德

##### 问题

容易造成 `underfitting` ，有可能一个属性都不会扩展出来。

#### 后剪枝 post-pruning

先将树在 `training set`上完全生长，然后从底向上进行剪枝

- 在 `validation set` 上对这一决策树进行测试，计算出一个准确度
- 从决策树底部选取一个属性，将该属性的划分替换为叶节点
  - 因为替换为叶节点所以，在该节点的判定就按照哪边的数据大就分为哪一类
- 替换为叶节点之后，再次应用 `validation set` 对其进行测试，计算出一个准确度
- 如果准确度有提升，则拿掉该节点
  - 否则保留

### 处理连续值

对于连续值如何选取分割点对当前数据进行切分

- 将某一连续属性值从小到大排序,如下 $$ a_1,a_2,a_3,\ldots,a_{V-1},a_V $$
- 则每次选取 $a_{i-1},a_i$ 之间的中间值，对数据集进行切分。则一共有 n-1 种选择

#### 处理缺失值

对于缺失值的处理，有两种情况

- 当数据的某一属性缺失时，如何进行最优属性的选择？
- 给定了最优属性的选择，当数据的某一属性缺失时，将该数据分到哪一个组？

##### 如何进行最优属性的选择

在有数据缺失的情况下，最优属性的选择只能基于**在该属性下**不缺失的数据，但是同时也要考虑到**在该属性下缺失数据**的多少，则计算公式如下
$$
Gain(D,a) = \frac{D_{未缺失数据}}{D_{全体数据}} Gain(D_{未缺失数据},a)
$$


#### 给定最优属性，如何将缺失属性的数据分组

将缺失属性的数据分入每一个组，但是改变其权重为
$$
\frac{D_{第k属性分值的完整数据}}{D_{全体完整数据}}
$$
假设某一个属性的完整数据为 15 ，全体数据为 16，则有一个数据在此属性上缺失，且该属性有三个分值，三个分值的数据分别为7,5,3

则该缺失数据在三个分值下的权重分别为 $\lbrace \frac{7}{15},\frac{5}{15},\frac{3}{15} \rbrace$, 其他完整数据的权值为 1