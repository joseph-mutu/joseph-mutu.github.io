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



#### 几何分布 p(X>m)

抛骰子连抛 m 把也没有 6 点
$$
\begin{equation}
P(X>m)=\underbrace{\sum_{k=m+1}^{\infty}(1-p)^{k-1} p}=\underbrace{\frac{p(1-p)^{m}}{1-(1-p)} }=(1-p)^{m}
\end{equation}
$$

$$
\sum_{k=m+1}^{\infty}(1-p)^{k-1}p \Rightarrow p \sum_{k=m+1}^{\infty}(1-p)^{k-1} \\
\space \\
\Rightarrow p(1-p)^{m} \sum \limits_{k=1}^{\infty} (1-p)^k \\
\space \\
\Rightarrow p(1-p)^{m} {\frac{1}{1-(1-p)}}  = \frac{p(1-p)^{m}}{1-(1-p)}
$$

#### 二项分布的期望计算

利用了二项式定理的反用

##### 二项式定理

$$
(a+b)^n = \sum \limits_{k=0}^n C_{n}^k a^k b^{n-k} \\\space \\=  C_{n}^0 a^0 b^{n} +C_{n}^1 a^1 b^{n-1} + C_{n}^2 a^2 b^{n-2} + \ldots + C_{n}^n a^n b^{0}
$$

##### 期望计算

$$
E(X) = \sum_{x=0}^n x \cdot C_n^x p^{x} (1-p)^{n-x}
$$

目的是利用上式的二项式定理，实现从右到左的简化, 但是上式因为每个 $p^x (1-p)^{n-x}$ 都有不同的系数 x，先将 x 统一化

- 首先注意一点

$$
k C_{n}^k = k \frac{n!}{k!(n-k)!} = \frac{n!}{(k-1)!(n-k)!} = n \cdot \frac{(n-1)!}{(k-1)!(n-k)!} = n  C_{n-1}^{k-1} \tag{1}
$$

- 将 E(X) 展开，注意到第一项为 0，所以将第一项去除


$$
E(X) = \underbrace{0 \cdot C_n^0 p^0 (1-p)^n + 1}_0 \cdot C_n^1 p (1-p)^{n-1} + \ldots \\\space \\E(X) = \sum \limits_{x=1}^n x \cdot C_n^x p^{x} (1-p)^{n-x} \tag{2}
$$

- 利用 (1) 式，将 (2) 化简

$$
E(X) = n \sum \limits_{x=1}^n C_{n-1}^{x-1} p^x (1-p)^{n-x}
$$

- 将每一项提取出一个 p 得到

$$
E(X) = np \sum \limits_{x=1}^n C_{n-1}^{x-1} p^{x-1} (1-p)^{n-x}
$$

如今上式，x 从 x = 1 处开始，**将 x = x+1，则循环从 x = 0出开始进行**

- E(X) 如下

$$
E(X) = np \underbrace{\sum \limits_{x=0}^n C_{n-1}^{x} p^{x} (1-p)^{n-x-1}}_{((1-p)+p)^{n-1}} \\\space \\ = np (1-p+p)^{n-1} = nE(X) = np \underbrace{\sum \limits_{x=0}^n C_{n-1}^{x} p^{x} (1-p)^{n-x-1}}_{((1-p)+p)^{n-1}} \\\space \\ = np (1-p+p)^{n-1} = np
$$









#### 泊松分布

有以下,下列为泊松分布的概率密度函数，则相加（积分）为 1
$$
e^{-\lambda} \sum _{k=0}^\infty \frac{\lambda^k}{k!} = 1
$$
