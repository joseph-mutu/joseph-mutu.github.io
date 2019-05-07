---
layout:     post   				    # 使用的布局（不需要改）
title:      Variational Inference 				# 标题 
subtitle:   VI          #副标题
date:       2019-05-04 				# 时间
author:     WYX 						# 作者
header-img: img/3.1.png 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - VI,PGM

---



# Variational Inference

 本文推导过程基本遵循“机器学习-白板推导系列（十二）-变分推断（Variational Inference）”

---



在推导过程中



$$
X: Observed \space Data \\
\space\\
Z: Latent  \space Variables \space +  \space Parameters \\
\space \\
(X,Z)： Compelte \space Data
$$



建立似然函数



$$
P(Z|X) = \frac{P(X,Z)}{P(X)} \\
\space \\
\Rightarrow P(X) = \frac{P(X,Z)}{P(Z|X)}
$$



对 P(X) 取对数可得：

- note: 下式引入的 q(z) 为隐变量以及参数的概率密度函数，概率密度函数的积分为 1






$$
logP(X) = logP(X,Z) - logP(Z|X)\\
\space \\
对上式进行变形，同时添加一个 q(Z)\\
\space \\
logP(X) = log\frac{P(X,Z)}{q(Z)} -  log\frac{P(Z|X))}{q(Z)} \\
\space \\
两边同时对 Z 求期望
$$





$$
左边 = \int _Z logP(X) q(Z) dz = logP(X) \underbrace{\int_Z q(Z) dz}_1 = logP(X) \tag{1}
$$



$$
右边 = \int_Z log\frac{P(X,Z)}{q(Z)} q(Z)dz - \int_Z log\frac{P(Z|X)}{q(Z)}q(Z)dz \tag{2}
$$



结合 (1) 和 (2) 可得：



$$
logP(X) = \underbrace{\int_Z log\frac{P(X,Z)}{q(Z)} q(Z)dz}_{ELBO} \underbrace{-\int_Z log\frac{P(Z|X)}{q(Z)}q(Z)dz}_{\mathbb{KL}(q(Z)||P(Z|X))} \\
\space \\
\Rightarrow logP(X) = \underbrace{ELBO}_{\mathbb{L(q)}} + \underbrace{\mathbb{KL}(q||P)}_{\geq 0}
$$



与 EM 算法类似，目标为最大化 P(X) ,使得引入的概率密度函数 q(Z) 近似等于 p(Z\|X)

- 在 EM 算法中，令 q(Z) = $P$(Z\|X) 
- 变分推断应用的场景为，后验概率过于复杂难以得到，所以 P(Z\|X) 未知
  
  - $\mathbb{L}(q)$ 称为变分
  - 在变分推断中，问题转换为寻找到一个 q(Z) 使得 $\mathbb{L(q)}$ 达到最大值，从而使得 $\mathbb{KL}$ 达到最小值
  
  



令



$$
q(Z) = \prod_{i=1}^M q_i(Z_i) \tag{3}
$$



注意因为关于 Z 的概率分布是自己引入的，所以可以自行定义。在这里将q(Z) 整体分成 M 份，但是每一个份可能也会存在多个维度

将 （3）代入变分



$$
\mathbb{L}(q) = \underbrace{\int_Z logP(X,Z)q(Z)dz}_{part1} - \underbrace{\int_Zq(Z) logq(Z)dz}_{part2} \\
\space \\
\mathbb{L}(q) = \underbrace{\int_Z logP(X,Z)\prod_{i=1}^M q_i(Z_i)dz}_{part1} - \underbrace{\int_Z\prod_{i=1}^M q_i(Z_i) log \prod_{i=1}^M q_i(Z_i) dz}_{part2}
$$



以下对于part1 和 part2 进行分别求解：





$$
part1 = \int _{z_j}q_j(Z_j) \underbrace{\prod_{i \neq j}^M q_i(Z_i) logP(X,Z) d_{z_1}d_{z_2}\ldots d_{z_M}}_{\mathbb{E}_{\prod_{i \neq j}^M q_i(Z_i)}[logP(X,Z)]} d_{z_j} \\
\space \\
part1 = \int _{z_j}q_j(Z_j) \mathbb{E}_{\prod_{i \neq j}^M q_i(Z_i)}[logP(X,Z)] d_{Z_j}
$$





以下将part2 进行展开，并提取其中第一项作为例子






$$
part2 = \int_Z\prod_{i=1}^M q_i(Z_i) log \prod_{i=1}^M q_i(Z_i) dZ \\
\space \\
= \int_Z q_1(Z_1)q_2(Z_2)\ldots q_M(Z_M) [logq_1(Z_1)+logq_1(Z_1)+\ldots +logq_M(Z_M)]dZ
$$





提取第一项



$$
= \int_{Z_1 Z_2 \ldots Z_M }q_1(Z_1)q_2(Z_2)\ldots q_M(Z_M) [logq_1(Z_1)]dZ_1dZ_2\ldots dZ_M \\
\space \\
= \int_{Z_1} q_1(Z_1)logq_1(Z_1)dZ_1 \underbrace{\int_{Z_2}q_2(Z_2)dZ_2}_{1}\ldots\underbrace{\int_{Z_2}q_M(Z_M)dZ_M}_1 \\
\space \\
= \int_{Z_1}q_1(Z_1)logq_1(Z_1)
$$



则 part2 可以写为


$$
part2 = \sum_{i=1}^M \int_{Z_i} q_i(Z_i)dZ_i
$$


当只关心第 j 项的时候，part2 可以写为


$$
part2 = \int_{Z_j} q_j(Z_j)dZ_j + constant
$$


将 part1 和 part2 关于第 j 项进行合并


$$
\mathbb{L(q_j)} = \int _{z_j}q_j(Z_j) \mathbb{E}_{\prod_{i \neq j}^M q_i(Z_i)}[logP(X,Z)] d_{Z_j} - \int_{Z_j} q_j(Z_j)dZ_j + C
$$


part1 和 part2 的关系是减，但是对于 constant 来说，正负不影响





进行如下定义：




$$
log\tilde{P}(X,Z) = \mathbb{E}_{\prod_{i \neq j}^M q_i(Z_i)}[logP(X,Z)] d_{Z_j}
$$


则关于第 j 项的更新可以重新写为一个 $\mathbb{KL}$ 散度


$$
\mathbb{L(q_j)} = \int_{Z_j} q_i(Z_i) log \frac{log\tilde{P}(X,Z)}{q_i(Z_i)} dZ_j \\
\space \\
= -\mathbb{KL}(q\|\tilde{P}) \leq 0
$$


因为原始的似然函数与 $\mathbb{L(q)}$ 存在如下关系：




$$
\Rightarrow logP(X) = \underbrace{ELBO}_{\mathbb{L(q)}} + \underbrace{\mathbb{KL}(q||P)}_{\geq 0}
$$


最大化 ELBO，就是最小化其 $-\mathbb{KL}$




# Reference

[b站-机器学习-白板推导](<https://www.bilibili.com/video/av32047507/?p=2>)