---
layout:     post   				    # 使用的布局（不需要改）
title:      HMM 中前向后向以及维特比与 Sum-Product 以及 Max-Product 的联系			# 标题 
subtitle:   sum-product, max-product           #副标题
date:       2020-04-20 				# 时间
author:     WYX 						# 作者
header-img: img/4.5.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - hmm

---

---

本文记录 HMM 中前向算法后向算法与 Sum-product 以及 维特比算法与 Max-product 之间的联系

---



下面两种方法

- Sum-Product 根据统计学习方法中 P177 页的设定重新计算
- Max - Product 根据统计学习方法中 P186 页中设定重新计算

# 前向后向与 Sum-Product

Sum-Product 为一个的核心思想就是记录结果再利用，沿着图将所有的消息计算下来，然后对 $h_1,h_2,h_3$ 做 Marginalize ，最后得到的结果就是
$$
p(x_1 = 红, x_2 = 白,x_3 = 红)
$$


<center>
    <img src = "https://github.com/joseph-mutu/Pics/raw/master/%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0/p177.jpg" width = "500"/>
</center>

上述为模型参数，要进行 Sum-Product/ Max-Product 首先将 HMM 有向图(贝叶斯网络) 转化为因子图形式

因子图计算时分为两种形式

- 从因子到节点
- 从节点到因子



<center>
    <img src = 'https://github.com/joseph-mutu/Pics/raw/master/%E8%B4%9D%E5%8F%B6%E6%96%AF%E6%8E%A8%E6%96%AD/factor-hmm.png' width = '400'/>
</center>

## 初始化

上述 $f_0$ 为初始节点的因子节点，意即消息从 $f_0$ 开始传递，而 $x_1,x_2,x_3$ 为叶子节点

初始化要做两件事

- 初始化 $\mu_{f_0\rightarrow h_1}(h_1) = f(h_1) $ 
  - 此处 $f(h_1)$ 为模型中的初始概率  
  - 为了统计表示无向图以及有向图，均使用 $f$ 表示节点之间的函数
  - 在有向图中节点之间的函数为条件概率
  - 在无向图中节点之间的函数为定义的势函数
- 初始化叶子节点
  - $\mu_{x_1\rightarrow f_1}(x_1) = 1 $

---

Note:

此题已经给定了 `Evidence` 。即指定观测值 `x_1,x_2,x_3` 为 `红,白,红`

此种情况下 也就不用进行消息传递了

- $\mu_{x_1\rightarrow f_1}(x_1= 白) = 0$
- $\mu_{x_2\rightarrow f_1}(x_2= 红) = 0$
- $\mu_{x_3\rightarrow f_1}(x_3= 白) = 0$

----

### 消息传递

#### 初始化

**叶子节点**


$$
\mu_{x_1\rightarrow f_1}(x_1) = 1  \\
\space \\
\Rightarrow \mu_{x_1\rightarrow f_1}(x_1 =红) = 1 \\
\space \\
\mu_{x_1\rightarrow f_3}(x_2) = 1  \\
\space \\
\Rightarrow \mu_{x_2\rightarrow f_3}(x_2 =白) = 1 \\
\space \\
\mu_{x_3\rightarrow f_5}(x_3) = 1  \\
\space \\
\Rightarrow \mu_{x_3\rightarrow f_5}(x_3 =红) = 1 \\
\space \\
$$


叶子节点的因子节点**
$$
\mu_{f_0\rightarrow h_1}(h_1) = f(h_1)  \\
\space \\
\mu_{f_0\rightarrow h_1}(h_1=1) = 0.2 \\
\space \\
\mu_{f_0\rightarrow h_1}(h_1=2) = 0.4 \\
\space \\
\mu_{f_0\rightarrow h_1}(h_1=3) = 0.4 \\
\space \\
$$

#### **$f_1 \rightarrow h_1$**

$$
\mu_{f_0\rightarrow h_1}(h_1) = f(h_1) \\
\space \\
\mu_{f_0\rightarrow h_1}(h_1=1) = p(x1 = 红| h_1 =1) = 0.5\\
\space \\
\mu_{f_0\rightarrow h_1}(h_1=2) = p(x1 = 红| h_1 =2) = 0.4\\
\space \\
\mu_{f_0\rightarrow h_1}(h_1=3) = p(x1 = 红| h_1 =3) = 0.7\\
$$

#### **$h_1 \rightarrow f_2$**

节点到因子的计算


$$
\mu_{h_1\rightarrow f_2}(h_1) = \prod \mu_{f_0\rightarrow h_1}(h_1) \mu_{f_0\rightarrow h_1}(h_1)  \\
$$


$h_1$ 存在三种取值，三种取值分开计算


$$
\mu_{h_1\rightarrow f_2}(h_1=1) = \mu_{f_0\rightarrow h_1}(h_1=1) \mu_{f_0\rightarrow h_1}(h_1=1) \\
\space \\
= 0.2 * 0.5 = 0.1
$$

$$
\mu_{h_1\rightarrow f_2}(h_1=2) = \mu_{f_0\rightarrow h_1}(h_1=2) \mu_{f_0\rightarrow h_1}(h_1=2) \\
\space \\
= 0.4 * 0.4 = 0.16
$$

$$
\mu_{h_1\rightarrow f_2}(h_1=3) = \mu_{f_0\rightarrow h_1}(h_1=3) \mu_{f_0\rightarrow h_1}(h_1=3) \\
\space \\
= 0.4 * 0.7  = 0.28
$$

#### $f_2 \rightarrow h_2$

因子到节点的计算


$$
\mu_{f_2\rightarrow h_2}(h_2) = \sum_{h_2} p(h_1,h_2)\mu_{h_1\rightarrow f_2}(h_1)
$$

$$
\mu_{f_2\rightarrow h_2}(h_2) = \\
\space \\
p(h_2=1|h_1=1)\mu_{h_1\rightarrow f_2}(h_1=1) + \\
\space \\
p(h_2=1|h_1=2))\mu_{h_1\rightarrow f_2}(h_1=2) +\\
\space\\
p(h_2=1|h_1=3)\mu_{h_1\rightarrow f_2}(h_1=3)\\
\space \\
= 0.5 * 0.1 + 0.3 * 0.16 + 0.3 * 0.28
= 0.154
$$



---

$$
\mu_{f_2\rightarrow h_2}(h_2) = \\
\space \\
p(h_2=2|h_1=1)\mu_{h_1\rightarrow f_2}(h_1=1) + \\
\space \\
p(h_2=2|h_1=2))\mu_{h_1\rightarrow f_2}(h_1=2) +\\
\space\\
p(h_2=2|h_1=3)\mu_{h_1\rightarrow f_2}(h_1=3)\\
\space \\
= 0.2 * 0.1 + 0.5 * 0.16 + 0.3 * 0.28 \\
\space \\
= 0.184
$$

---

$$
\mu_{f_2\rightarrow h_2}(h_2) = \\
\space \\
p(h_2=3|h_1=1)\mu_{h_1\rightarrow f_2}(h_1=1) + \\
\space \\
p(h_2=3|h_1=2))\mu_{h_1\rightarrow f_2}(h_1=2) +\\
\space\\
p(h_2=3|h_1=3)\mu_{h_1\rightarrow f_2}(h_1=3)\\
\space \\
= 0.3 * 0.1 + 0.2 * 0.16 + 0.5 * 0.28 \\
\space \\
= 0.202
$$

#### **$f_3 \rightarrow h_2$**

$$
\mu_{f_3\rightarrow h_2}(h_2) = f(h_2) \\\space \\\mu_{f_3\rightarrow h_2}(h_2=1) = p(x2 = 红| h_2 =1) = 0.5\\\space 
\\\mu_{f_3\rightarrow h_2}(h_2=2) = p(x2 = 红| h_2 =2) = 0.6\\\space \\\mu_{f_3\rightarrow h_2}(h_2=3) = p(x2 = 红| h_2 =3) = 0.3\\
$$

#### $h_2 \rightarrow f_4$

节点到因子的计算


$$
\mu_{h_2\rightarrow f_4}(h_2) = \prod \mu_{f_2\rightarrow h_2}(h_2) \mu_{f_3\rightarrow h_2}(h_2)  \\
$$


$h_2$ 存在三种取值，三种取值分开计算


$$
\mu_{h_2\rightarrow f_4}(h_2=1) = \mu_{f_2\rightarrow h_2}(h_2=1) \mu_{f_3\rightarrow h_2}(h_2=1) \\
\space \\= 0.154 * 0.5 = 0.077
$$

$$
\mu_{h_2\rightarrow f_4}(h_2=1) = \mu_{f_2\rightarrow h_2}(h_2=1) \mu_{f_3\rightarrow h_2}(h_2=1) \\
\space \\= 0.184 * 0.6 = 0.1104
$$

$$
\mu_{h_2\rightarrow f_4}(h_2=1) = \mu_{f_2\rightarrow h_2}(h_2=1) \mu_{f_3\rightarrow h_2}(h_2=1) \\
\space \\= 0.202 * 0.3 = 0.0606
$$

#### $f_4 \rightarrow h_3$

因子到节点的计算


$$
\mu_{f_4\rightarrow h_3}(h_3) = \sum_{h_3} p(h_2,h_3)\mu_{h_2\rightarrow f_4}(h_2)
$$

$$
\mu_{f_4\rightarrow h_3}(h_3) = \\
\space \\p(h_3=1|h_2=1)\mu_{h_2\rightarrow f_4}(h_2=1) + \\
\space \\p(h_3=1|h_2=2))\mu_{h_2\rightarrow f_4}(h_2=2) +\\
\space\\p(h_3=1|h_1=3)\mu_{h_1\rightarrow f_2}(h_2=3)\\
\space \\
= 0.5 * 0.077 + 0.3 * 0.1104 + 0.2 * 0.0606= 0.08374
$$

---

$$
\mu_{f_4\rightarrow h_3}(h_3) = \\
\space \\p(h_3=2|h_2=1)\mu_{h_2\rightarrow f_4}(h_2=1) + \\
\space \\p(h_3=2|h_2=2))\mu_{h_2\rightarrow f_4}(h_2=2) +\\
\space\\p(h_3=2|h_1=3)\mu_{h_1\rightarrow f_2}(h_2=3)\\
\space \\
= 0.2 * 0.077 + 0.5 * 0.1104 + 0.3 * 0.0606= 0.08878
$$

---

$$
\mu_{f_4\rightarrow h_3}(h_3) = \\
\space \\p(h_3=3|h_2=1)\mu_{h_2\rightarrow f_4}(h_2=1) + \\
\space \\p(h_3=3|h_2=2))\mu_{h_2\rightarrow f_4}(h_2=2) +\\
\space\\p(h_3=3|h_1=3)\mu_{h_1\rightarrow f_2}(h_2=3)\\
\space \\
= 0.3 * 0.077 + 0.2 * 0.1104 + 0.5 * 0.0606=0.07548
$$



#### $f_5 \rightarrow h_3$

$$
\mu_{f_5\rightarrow h_3}(h_3) = f(h_3) \\
\space \\
\mu_{f_5\rightarrow h_3}(h_3=1) = p(x3 = 红| h_3 =1) = 0.5\\
\space \\
\mu_{f_5\rightarrow h_3}(h_3=2) = p(x3 = 红| h_3 =2) = 0.4\\
\space \\
\mu_{f_5\rightarrow h_3}(h_3=3) = p(x3 = 红| h_3 =3) = 0.7\\
\space \\
$$

#### $p(h_3)$

$$
p(h_3) = \prod \mu_{f_0\rightarrow h_1}(h_1) \mu_{f_0\rightarrow h_1}(h_1)  \\
$$

$$
p(h_3=1) =  \mu_{f_4\rightarrow h_3}(h_3=1) *\mu_{f_5\rightarrow h_4}(h_4 = 1)\\ 
=  0.08374 * 0.5 = 0.04187 \\
$$

$$
p(h_3=1) =  \mu_{f_4\rightarrow h_3}(h_3=1) *\mu_{f_5\rightarrow h_4}(h_4 = 1)\\ 
=  0.08878 * 0.4 = 0.035512 \\
$$

$$
p(h_3=1) =  \mu_{f_4\rightarrow h_3}(h_3=1) *\mu_{f_5\rightarrow h_4}(h_4 = 1)\\ 
=  0.07548 * 0.7 = 0.052836 \\
$$

#### 终止

将 $p(h_3)$ 进行 Marginalize 则，整个联合概率
$$
\sum _{h_1,h_2,h_3}p(x_1=红,x_2 = 白,x_3 = 红,h_1,h_2,h_3) \\
\space \\
\Rightarrow p(x_1 = 红,x_2 = 白, x_3 = 红)
$$


# 维特比与Max-Product

<center>
    <img src = "https://github.com/joseph-mutu/Pics/raw/master/%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0/p186.jpg" width = '500'/>
</center>







<center>
    <img src = "https://github.com/joseph-mutu/Pics/raw/master/%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0/p186ans.jpg" width = '500'/>
</center>



 