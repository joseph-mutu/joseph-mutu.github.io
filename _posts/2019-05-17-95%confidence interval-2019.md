---
layout:     post   				    # 使用的布局（不需要改）
title:      confidence interval 				# 标题 
subtitle:   confidence interval, central limit theory          #副标题
date:       2019-05-10 				# 时间
author:     WYX 						# 作者
header-img: img/wuxia3.png 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - confidence interval

---



# 95% 置信区间

本文基本遵循知乎“马同学答案 -- 如何理解 95% 置信区间”

## 标准正态分布

若样本独立同分布（iid）服从正态分布：



$$
X \sim N(\mu,\delta^2) 
$$



则样本期望及方差为：

$$
E(X) = \mu,~ Var(X) = \delta^2
$$



全体样本服从同一分布，则意味着样本期望与方差相同（针对同一正态分布来说）

### 转化标准正态分布

当样本服从任意正态分布



$$
X \sim N(\mu,\delta^2)\\
\space
\\
\Rightarrow \frac{X-\mu}{\delta} \sim N(0,1)
$$



均可以转化成为标准正态分布之后再进行计算

![](https://ae01.alicdn.com/kf/HTB11YR7XlCw3KVjSZFlq6AJkFXaa.jpg)

则存在



$$
设 X \sim N(\mu,\delta^2)，则 \\
\space \\
\\
P(|X-\mu| < k\delta) = \Phi(k) - \Phi(-k) = 2\Phi(k) - 1
$$



### 正态分布样本均值的期望以及方差



$$
\bar{X} = \frac{X_1 + X_2 + \ldots + X_n}{n}
$$



则其均值的期望为：


$$
E(\bar{x}) = E(\frac{\sum_{i=1}^n x_i}{n}) = \frac{1}{n} E(\sum_{i=1}^n x_i) \\
\space \\
= \frac{1}{n} \underbrace{\sum_{i=1}^n E(x_i)}_{期望的性质} = \mu
$$



则其均值的方差为：



$$
Var(\bar{X}) = Var(\frac{\sum_{i=1}^n x_i}{n}) \\
\space
\\
= \frac{1}{n^2} Var(\sum_{i=1}^n x_i)
$$


因为样本独立，则有



$$
 Var(\sum_{i=1}^n x_i) =  \sum_{i=1}^nVar(x_i) \\
 \space \\
 因为样本同分布，则存在
  \sum_{i=1}^nVar(x_i) = n\delta^2
$$



则原式可以写为：



$$
Var(\bar{X}) = \frac{1}{n^2} Var(\sum_{i=1}^n x_i) = \frac{\delta^2}{n} \\
\space \\
样本独立同分布 \Rightarrow Var(x_1+x_2+\ldots+x_n) = Var(x_1) + Var(x_2) + \ldots +Var(x_n)
$$



## 95% 置信区间

平均身高 145 为上帝视角，这个值无人知道。 95% 的置信区间就是根据某一个范围进行估计，使得真实值有 95% 的可能被囊括其中（实际中，使用 $\hat{\mu}$ 模拟 $\mu$）

![](https://ae01.alicdn.com/kf/HTB1ePu_aL5G3KVjSZPxq6zI3XXao.jpg)

假设人群的身高服从：


$$
X \sim N(\mu,\delta^2)
$$


现在根据对人群的采样样本，对 $\mu$ 进行估计


$$
X = \frac{x_1 + x_2 + \ldots + x_n}{n} \sim N(\mu,\frac{\delta^2}{n})
$$


![](https://ae01.alicdn.com/kf/HTB1kmy_aNiH3KVjSZPfq6xBiVXaz.jpg)



这里的 1.96 为查表结果，因为我们要构造面积为 0.95 的区间，则右半边空白的概率为 0.25，左半边空白的概率为 0.25


$$
P(X<a) = 0.975
$$


注意此时假设 X 服从标准正态分布（需要进行转换），查标准正态分布表得到， a = 1.96

则此时有


$$
P(X < 1.96) = 0.975 \\
\space 
\\
P(\frac{X - \mu}{\delta^2/n} < 1.96) = 0.95
$$


![](https://ae01.alicdn.com/kf/HTB1FznfaRGw3KVjSZFwq6zQ2FXan.jpg)



因为不知道 $\mu$ 的真实值，所以使用样本的均值 $\hat{\mu}$ 来代替 $\mu$








# Reference

[如何理解 95% 置信区间](<https://www.zhihu.com/question/26419030>)