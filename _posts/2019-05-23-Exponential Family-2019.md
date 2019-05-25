---
layout:     post   				    # 使用的布局（不需要改）
title:      Exponential Family 				# 标题 
subtitle:   Exponential Family          #副标题
date:       2019-05-10 				# 时间
author:     WYX 						# 作者
header-img: img/wuxia0.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - Exponential Family,PGM

---



# 指数分布族

本文脉络大体跟着两个视频走

- 机器学习-白板推导系列（八）- 指数族分布
- 徐亦达机器学习指数族分布及变分

一些理论理解则遵循

- LDA 漫游指南

---

![1558622547599](C:\Users\mutudeh\AppData\Roaming\Typora\typora-user-images\1558622547599.png)

## 数学期望与积分

#### 随机变量的定义

> 假设一个变量的取值依赖于随机现象的结果，则称此变量为随机变量

一般大写字母 $X$ 表示随机变量，其相应的取值使用小写字母 $x$ 表示

#### 数学期望

> 期望$\mathbb{E}$： 随机变量的平均值

假设一共做 N 次试验，记录每次试验 X 的取值。在 N 次试验中，其中 $N_1$ 次取到 $x_1$, $N_2$ 次取到 $x_2$,$\ldots$,则在 N 次试验中，X 的取值一共为 $x_1N_1 + x_2N_2+\ldots+x_NN_N$, 将平均每次试验 **X 的取值记为 $\mat**hbb{E}(X)$, 则存在


$$
\mathbb{E}(X) = \frac{x_1N_1 +x_2N_2 + \ldots + x_NN_N }{N}
$$


**根据伯努利大数定理，随着事件 A（上式中的 N）发生的次数不断增大，其频率将趋近于概率**

![](https://ae01.alicdn.com/kf/HTB1l2NXaa5s3KVjSZFNq6AD3FXa9.jpg)

则可以得到


$$
\frac{N_1}{N} \approx p_1
$$


则期望可以表示为


$$
\mathbb{E}(X) = \sum_1^{\infin} x_ip_i
$$




#### 看到数学期望就想到积分

假设现在要对关于变量 X 的一个函数 **g(X)** 关于 **x 的概率密度函数 p(X)**求取期望，则可以写为


$$
\mathbb{E}_{p(x)}g(X) = \int_{-\infty}^{+\infty} p(x)g(x) dx
$$




离散型的随机变量可以写为


$$
\mathbb{E}_{p(x)}g(X)  = \sum_x p(x)g(x)
$$


如果是二维随机变量(X,Y)，则可以写为


$$
\mathbb{E}_{p(x,y)}g(X,Y) = \int_{-\infty}^{+\infty} \int_{-\infty}^{+\infty} f(x,y)g(x,y) dx dy
$$


## 指数分布族的形式

指数分布族分布的概率密度函数具备以下形式


$$
p(x|\eta) = h(x)~exp \lbrace\eta^T\tau(x) - A(\eta)\rbrace
$$


#### $A(\eta) $ 被称为 **log partition function**, 配分函数

配分函数在计算上的意义就是将函数归一化为 1。在概率图的计算中，后验概率因为直接计算难以得到，所以会用别的函数对后验进行模拟


$$
P(X|\theta) = \underbrace{\frac{1}{Z}}_{归一化因子}\hat{P}(X|\theta)  \\
\space \\
Z = \int_x \hat{P}(x|\theta) dx
$$


因为 $\hat{P}(X|\theta)$ 无法保证概率密度函数的性质，也就是积分为 1，则使用 Z 对其进行归一化处理

##### 配分函数形式推导


$$
P(X|\eta) = h(X)\cdot exp\lbrace \eta^T\tau(X)\rbrace \cdot exp\lbrace -A(\eta) \rbrace \\
\space \\
= \underbrace{\frac{1}{exp\lbrace A(\eta) \rbrace}}_Z  \underbrace{h(X)\cdot exp\lbrace \eta^T\tau(X)\rbrace}_{\hat{P(X|\eta)}} \\
\space \\
\Rightarrow exp\lbrace A(\eta) \rbrace = z \\
\space \\
A(\eta) = log ~ z (log~ partition ~function) \tag{1}
$$


#### 充分统计量

>  统计量表示对样本的函数，比如，期望或者方差。而充分统计量则表示，利用当前的这一统计量，就可以完全反映出总体所有的信息

##### 充分统计量实例

以正态分布举例


$$
N(\mu,\delta^2): p(x) = \frac{1}{\sqrt{2\pi \delta^2}} exp  \lbrace - \frac{(x - \mu)^2}{2 \delta^2} \rbrace \\
\space \\
\mathbb{E}(X) = \mu \\
\space \\
Var(X) = \delta^2
$$


假设存在一组数据 $X = \lbrace x_1,x_2,\dots,x_N \rbrace$根据极大似然估计可以得到对于期望以及方差的极大似然估计


$$
\hat{\mu} = \bar{x} \\
\space \\
\hat{\delta} = \frac{1}{n} \sum_{i=1} ^N (x_i - \bar{x}) ^2
$$
则我们可以设计一个统计量 $\tau(x)$ 为充分统计量


$$
\tau(x) = 
\left[
\begin{matrix}
	\sum_i^N x_i \\
	\sum_i ^N x_i^2
\end{matrix}
\right]
$$


我们可以从上式中的充分统计量中构造出**一摸一样的概率密度函数**，可以估计其**期望**以及**方差**



#### 正态分布的指数族分布形式

以下为一般的正态分布形式，其中 $\theta$ 为参数
$$
p(x|\theta) = \frac{1}{\sqrt{2\pi \delta^2}} exp \lbrace \frac{(x - \mu)^2}{2 \delta^2} \rbrace,~\theta = (\mu,\delta^2)
$$


以下为一般的指数族分布形式，其中 $\eta$ 为参数


$$
p(x|\eta) = h(x)~exp \lbrace\eta^T\tau(x) - A(\eta)\rbrace
$$


将正态分布的形式展开


$$
p(x|\theta) = exp\lbrace log(2\pi\delta^2)^{-\frac{1}{2}} \rbrace exp \lbrace -(x^2 - 2x\mu + \mu^2) \frac{1}{2 \delta^2} \rbrace \\
\space \\
\\
p(x|\theta) = exp\lbrace log(2\pi\delta^2)^{-\frac{1}{2}} \rbrace exp \lbrace     \underbrace{ \left [ \begin{matrix} \frac{\mu}{\delta^2} & -\frac{1}{2\delta^2} \end{matrix} \right]}_{\eta^T} \underbrace{\left [ \begin{matrix} x \\ x^2 \end{matrix} \right]}_{\tau(x)} - \frac{\mu^2}{2\delta^2} \rbrace \\
\space \\
p(x|\theta) = exp \lbrace      \underbrace{ \left [ \begin{matrix} \frac{\mu}{\delta^2} & -\frac{1}{2\delta^2} \end{matrix} \right]}_{\eta^T} \underbrace{\left [ \begin{matrix} x \\ x^2 \end{matrix} \right]}_{\tau(x)} - \underbrace{(\frac{\mu^2}{2\delta^2} + \frac{1}{2} log(2\pi\delta^2)) }_{A(\eta)} \rbrace
$$


根据对应的指数分布族对应的形式，我们可以得到参数 $\theta$ 与参数 $\eta$ 之间的关系


$$
\eta = \left( \begin{matrix} \eta_1 \\ \eta_2 \end{matrix} \right) = \left( \begin{matrix} \frac{\mu}{\delta^2} \\ -\frac{1}{2\delta^2}\end{matrix} \right)
$$


则可以得到


$$
\left\{
\begin{aligned}
\eta_1 &= \frac{\mu}{\delta^2} \\
\eta_2 &=  -\frac{1}{2\delta^2}
\end{aligned}
\right. ~ \Rightarrow 
\left\{
\begin{aligned}
\mu =& -\frac{\eta_1}{2\eta_2} \\
\delta^2 =&  -\frac{1}{2\eta_2}
\end{aligned}
\right.
$$


则根据上式对应关系，log 配分函数 $A(\eta)$ 可以写为


$$
A(\eta) = \frac{\mu^2}{2\delta^2} + \frac{1}{2} log(2\pi\delta^2)) \\
\space \\

\Rightarrow -\frac{\eta_1^2}{4\eta_2} + \frac{1}{2} log(- \frac{\pi}{\eta_2})
$$




#### 充分统计量与配分函数之间的关系

由之前推导 {1} 可得到配分函数等于:


$$
P(X|\eta) = \frac{1}{exp\lbrace A(\eta) \rbrace}  h(X)\cdot exp\lbrace \eta^T\tau(X)\rbrace \\
\space \\
两边同时对 X 求积分\\
\int_X P(X|\eta)dX = \int_X \frac{1}{exp\lbrace A(\eta) \rbrace}  h(X)\cdot exp\lbrace \eta^T\tau(X)\rbrace dX \\
\space \\
\Rightarrow 1 = \frac{1}{exp\lbrace A(\eta) \rbrace}\int_Xh(X)\cdot exp\lbrace \eta^T\tau(X)\rbrace dX \\
\space \\
\Rightarrow exp\lbrace A(\eta) \rbrace = \int_Xh(X)\cdot exp\lbrace \eta^T\tau(X)\rbrace dX
$$


现在对 $\eta$ 进行求导，则可以得到


$$
\frac{\partial expA(\eta)}{\partial \eta} = \frac{\partial \int_Xh(X)\cdot exp\lbrace \eta^T\tau(X)\rbrace dX}{\partial \eta} \\
\space \\
积分求导符号互换顺序
\\
expA(\eta)A'(\eta) =  \int_X\frac{  \partial h(X)\cdot exp\lbrace \eta^T\tau(X)\rbrace }{\partial \eta}dX \\
\space \\
A'(\eta) =   \int_X \underbrace{h(X) exp(\eta^T \tau(X) - A(\eta))}_{P(X|\eta)} \tau(X) dX \\
\space \\
= \int_X P(X|\eta) \tau(X) dX \\
\space \\
= \mathbb{E}_{P(X|\eta)}[\eta(X)]
$$




#### 共轭推导


$$
P(\beta|X) = \frac{ \overbrace{P(X|\beta))}^{似然函数} \overbrace{P(\beta)}^{先验}  }  {P(X)}
$$


因为我们现在求的是关于参数 $\beta$ 的后验概率，在给定 X 的情况下， P(X) 是一个常数。

**Note:**

注意 P(X) 也是一个概率分布，是离散型的，如下所示


$$
P(X) = \lbrace x_1,x_2,x_3,\ldots X_N\rbrace, \sum_i^{N}x_i = 1
$$
每一个 $X_i$ 代表一个概率值，则在给定 X 的情况下，它是一个常数

---

则后验概率可以写为正比的形式


$$
P(\beta|X) \propto P(X|\beta) P(\beta)
$$


将其中的先验和似然均写为指数族分布的形式


$$
P(X|\beta) = h(\beta)exp \lbrace \tau(X)\eta(\beta)^T - A(\beta)  \rbrace
$$


**Note:** 其中 $\tau(X)$ 为类似{X,X^2} 之类的充分统计量，$\eta(\beta)$ 为参数 $\beta$ 的函数，$A(\beta)$ 同样也为参数 $\beta $的函数

先验可以写为：


$$
P(\beta|\alpha) = h(\alpha)exp \lbrace \tau(\beta)\eta(\alpha)^T - A(\alpha)  \rbrace
$$


**Note**：其中 $\alpha$ 为先验分布的参数

则后验概率可以表示为


$$
P(\beta|X) \propto h(\beta)exp \lbrace \tau(X)\eta(\beta)^T - A(\beta)  \rbrace \cdot  h(\alpha)exp \lbrace \tau(\beta)\eta(\alpha)^T - A(\alpha)  \rbrace
$$


对于参数 $\beta$ 的后验来说，$\alpha$ 已经给定，则将 $h(\alpha)$ 以及 $A(\alpha)$ 认定为常数（给定 $\alpha$ ）


$$
P(\beta|X) \propto h(\beta) exp \lbrace \tau(X)\eta(\beta)^T - A(\beta)  \rbrace \cdot exp \lbrace \tau(\beta)\eta(\alpha)^T  \rbrace
$$


假设 $\beta$ 的充分统计量 $\tau(\beta)$ 可以写为两部分，$\alpha$ 则也可以分为两部分，如下所示


$$
\tau(\beta) = 
\left[
\begin{matrix}
\beta \\
-g(\beta)
\end{matrix}
\right] , \alpha = 
\left[
\begin{matrix}
\alpha_1 \\
\alpha_2
\end{matrix}
\right]
$$


则后验可以写为：


$$
P(\beta|X) \propto h(\beta) exp \lbrace \tau(X)\eta(\beta)^T - A(\beta) + \alpha_1\beta -\alpha_2g(\beta)  \rbrace
$$


在推导过程中，假设 $\beta$ 维度为 1（即由充分统计量 $\tau(X) $ 和参数 $\beta$ 构成的方程只含有 $\beta$ ，则


$$
P(\beta|X) \propto h(\beta) exp \lbrace \tau(X)\beta - A(\beta) + \alpha_1\beta -\alpha_2g(\beta)  \rbrace \\
\space \\
\propto h(\beta)exp \lbrace (\tau(X) + \alpha_1)\beta \underbrace{ - A(\beta) - \alpha_2 g(\beta)}_{无法合并，需要作出条件}   \rbrace
$$


对于后两个无法合并的项，作出限制，即 $A(\beta)  = g(\beta)$ ,也就是让**参数 $\beta $ 的先验的充分统计量的第二部分**要和 似然函数的 log 配分函数相等，从而先验和后验在给定似然的情况下才能共轭，写为如下形式，


$$
此时给定条件，A(\beta)  = g(\beta) \\
\space \\
P(\beta|X)\propto h(\beta)exp \lbrace (\tau(X) + \alpha_1)\beta  - A(\beta) - \alpha_2 A(\beta)  \rbrace \\
\space \\
\propto  h(\beta)exp \lbrace (\tau(X) + \alpha_1)\beta  - (1+\alpha_2)A(\beta) \rbrace
$$


则可得到其共轭形式



# Reference

[机器学习-白板推导系列（八）- 指数族分布](<https://www.bilibili.com/video/av33360526>)

[徐亦达-机器学习-youtube](<https://www.youtube.com/channel/UConITmGn5PFr0hxTI2tWD4Q>)

[LDA漫游指南](<https://yuedu.baidu.com/ebook/d0b441a8ccbff121dd36839a>)