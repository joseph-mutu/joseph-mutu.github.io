---
layout:     post   				    # 使用的布局（不需要改）
title:      Linear Gaussian Model				# 标题 
subtitle:   Bayesian Linear regression,multivariable gaussian           #副标题
date:       2019-10-16 				# 时间
author:     WYX 						# 作者
header-img: img/4.4.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - Bayesian Inference

---

---



# 线性高斯模型

假设存在数据 


$$
Data = \lbrace (x_1,y_1),(x_2,y_2)\ldots (x_N,y_N \rbrace \\
\space \\
x \in \mathbb{R}^P, y \in \mathbb{R} \\
\space \\
X = (x_1 ~  x_2 ~ \ldots ~ x_N )^T
$$


向量默认为`列向量`，则 $x$ 可以表示为矩阵的形式


$$
\left(
\begin{matrix}
x_{11} & x_{12} & x_{13} &\ldots & x_{1P} \\
x_{21} & x_{22} & x_{23} &\ldots & x_{2P} \\
\vdots & \vdots & \vdots & \vdots & \vdots \\
x_{N1} & x_{N2} & x_{N3} &\ldots & x_{NP}
\end{matrix}
\right)_{N \times P}
$$


设数据独立同分布，且服从于均值 $\mu$ 协方差矩阵为 $\Sigma$ 的高斯分布


$$
x_i \sim^{iid} N(\mu,\Sigma) \\
\space \\
\mu: P \times 1 ; \Sigma: P \times P \\
\space \\
\theta = (\mu ~\Sigma)
$$
假设在 **2 维**的情况下，对 $\theta$ 进行 MLE 估计，得到 $\theta_{MLE}$ 如下


$$
\mu_{MLE} = \frac{1}{N} \sum _{i=1} ^N x_i \\
\delta_{MLE}^2 = \frac{1}{N} \sum _{i=1} ^N (x_i - \mu_{MLE})^2
$$
$\delta_{MLE}$ 的估计值为**有偏估计**

## 有偏估计与无偏估计

对$\delta_{MLE}$ 求期望如下
$$
E[\delta_{MLE}] = E[\underbrace{\frac{1}{N} \sum _{i=1} ^N (x_i - \mu_{MLE})^2}] \tag{1}
$$
对上述括号内的式子，提出来并进行简化


$$
\frac{1}{N} \sum _{i=1} ^N (x_i - \mu_{MLE})^2 = \frac{1}{N} \sum _{i=1} (x_i^2 - 2x_i\mu_{MLE} + \mu_{MLE}^2) \\
\space 将 x_i 与 \mu_{MLE} 的关系带入 \\
= \frac{1}{N} \sum _{i=1} ^N x_i^2 - 2\mu_{MLE}\sum _{i=1} ^N x_i + N\mu_{MLE}^2 \\
= \frac{1}{N} \sum _{i=1} ^N x_i^2 - 2\mu_{MLE} \times N\mu_{MLE} + N\mu_{MLE}^2 \\
= \frac{1}{N} \sum _{i=1} ^N x_i^2 - 2N\mu_{MLE}^2 + N\mu_{MLE}^2 \\
= \frac{1}{N} \sum _{i=1} ^N x_i^2 - N\mu_{MLE}^2 \\
= \frac{1}{N} \sum _{i=1} ^N x_i^2 - \mu_{MLE}^2\tag{2}
$$
重写 (1) 式可以得到
$$
E[\delta_{MLE}^2] =  E[\frac{1}{N}\sum _{i=1} ^N x_i^2 - \mu_{MLE}^2 \tag{2}] \\
\space 添加一个 \mu^2 减去一个 \mu^2\\
=   E[\frac{1}{N}(\sum _{i=1} ^N x_i^2 - \mu^2) - (\mu_{MLE}^2 - \mu^2)] \\
= E[\frac{1}{N} \sum _{i=1} ^N (x_i^2 - \mu^2)] -  E[ (\mu_{MLE}^2 - \mu^2)] \\
= \frac{1}{N} \sum _{i=1} ^NE[x_i^2 - \mu^2] - (E[\mu_{MLE}^2] - E[\mu^2)]) \\
= \frac{1}{N} N Var(x_i) - (E[\mu_{MLE}^2] - \underbrace{\mu^2}_{E[\mu_{MLE}]^2}) \\
= Var(x_i)  - Var(\mu_{MLE}) \\
= \delta^2 - Var[ \frac{1}{N} \sum _{i=1} ^N x_i] \\= \delta^2 -  \frac{1}{N^2} \sum _{i=1} ^N Var(x_i) \\
= \delta^2 - \frac{1}{N^2}N \delta^2 = \delta^2 - \frac{1}{N} \delta^2 =\frac{N-1}{N}\delta^2
$$
最终可知从最大似然估计对 $\delta$ 的估计值为有偏估计，即其期望不为 $\delta^2$ 
$$
E[\delta_{MLE}^2] \neq \delta^2
$$
所以其无偏估计为
$$
\frac{N}{N-1}E[\delta_{MLE}^2] = \frac{N}{N-1}E[\frac{1}{N} \sum _{i=1} ^N (x_i - \mu_{MLE})^2] = \frac{1}{N-1}E[\frac{1}{N} \sum _{i=1} ^N (x_i - \mu_{MLE})^2]
$$
