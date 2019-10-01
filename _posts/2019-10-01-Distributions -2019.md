---
layout:     post   				    # 使用的布局（不需要改）
title:      Distributions 				# 标题 
subtitle:   Multinominal, Categorical, Beta, Dirichlet           #副标题
date:       2019-10-01 				# 时间
author:     WYX 						# 作者
header-img: img/5.12.png 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - Distributions

---



## 伯努利分布，二项式分布，多项式分布，多类别分布

- 抛一次**硬币**，正面向上的概率为伯努利分布
- 抛 n 次**硬币**，正面向上出现 **K** 次的概率为二项式分布
- 抛一次骰子，第 **i** 面朝上的概率为 Categorical 分布
- 抛 n 次**骰子**，其中 **1** 出现 $x_1$ 次，**2** 出现 $x_2$ 次，**3** 出现 $x_3$ 次，**4** 出现 $x_4$ 次，**5** 出现 $x_5$ 次，**6** 出现 $x_6$ 次的概率为多项式分布

### 多项式分布 pdf 推导

#### 多项式定理

有如下式子


$$
(x_1+x_2+x_3+x_4 + \ldots + x_k)^n
$$


根据多项式定理可以展开为


$$
(x_1 + x_2 + \ldots + x_k)^n = \underbrace{(x_1 + x_2 +x_3 \ldots + x_n)\times(x_1 + x_2 +x_3 \ldots + x_n)\times(x_1 + x_2 +x_3 \ldots + x_n) \ldots}_n
$$
给定其中 $x_1,x_2,x_3,\ldots x_k$ 各自在 n 中出现的次数，则将上式看做从 n 个式子中选取 $r_1$ 个 $x_1$，$r_2$ 个 $x_2$，$\ldots$ $r_k$ 个 $x_k$， 则可以写为


$$
C_n^{r_1} \times C_{n-r_1}^{r_2}\times C_{n-r_1-r_2}^{r_3} \ldots c_{n-r_1-\ldots-r_{k-1}}^{r_k} \\
\space \\
= \frac{n!}{r_1!(n-r_1)!} \times \frac{(n-r_1)!}{r_2! (n-r_1-r_2)!}\times\frac{(n-r_1-r_2)!}{r_3! (n-r_1-r_2-r_3)!} \times \ldots \times \frac{(n-r_1\ldots-r_{k-1})!}{r_k! \underbrace{(n-r_1-\ldots-r_k)}_0!} \\
\space \\
= \frac{n!}{r_1! \times r_2! \ldots r_k!}
$$


**Note**: 0! = 1

多项式分布的概率密度函数为
$$
P(X_1 = r_1, X_2 = r_2 , \ldots X_k = r_k) = \left\{
\begin{aligned}
 \frac{n!}{r_1! \times r_2! \ldots r_k!} x_1^{r_1}x_2^{r_2}x_3^{r_3}\ldots x_k^{x_k}\\
\end{aligned}
\right.
$$
上式中 $x_1,x_2 \ldots x_k$ 为参数，是抛骰子第一面朝上的概率 $x_1$ 第二面朝上的概率 $x_2$ ,$\ldots$ 第 k 面朝上的概率 $x_k$

而 $r_1,r_2,\ldots r_k$ 为第一面朝上出现的次数为 $r_1$, 第二面朝上出现的次数为 $r_2$ , $\ldots $ 第 k 面朝上出现的次数为 $r_k$