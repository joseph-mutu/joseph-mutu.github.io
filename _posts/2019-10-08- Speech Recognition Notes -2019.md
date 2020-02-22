---
layout:     post   				    # 使用的布局（不需要改）
title:      Speech Recognition				# 标题 
subtitle:   machine learning           #副标题
date:       2019-10-02 				# 时间
author:     WYX 						# 作者
header-img: img/wuxia4.png 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - decision trees, Bayesian Inference, Navie Bayes

---

---

本文对 “Speech Recognition” 中的要点进行记录

---

# 傅里叶级数与傅里叶变换

傅里叶级数可以将**周期性变换**的波形分解为一系列不同振幅（Amplitude），不同频率的正弦波的组合

<img src="C:\Users\mutudeh\AppData\Roaming\Typora\typora-user-images\1570728166293.png" alt="1570728166293" style="zoom:27%;" />

傅里叶变换可以将**非周期性变换**的波形进行分解。（周期为无穷）

<img src="C:\Users\mutudeh\AppData\Roaming\Typora\typora-user-images\1570728194825.png" alt="1570728194825" style="zoom:25%;" />

### 傅里叶变换与傅里叶级数分解波的区别

傅里叶级数对周期性波分解之后，为离散结果，横轴为频率，纵轴为振幅

<img src="C:\Users\mutudeh\AppData\Roaming\Typora\typora-user-images\1570728339327.png" alt="1570728339327" style="zoom:25%;" />

傅里叶变换对非周期性波分解之后，得到连续结果，横轴为频率，纵轴为振幅

<img src="C:\Users\mutudeh\AppData\Roaming\Typora\typora-user-images\1570728367062.png" alt="1570728367062" style="zoom:23%;" />

### 语谱图（spectrum）与上述的关系

如下图，语谱图的横坐标为时间，纵坐标为频率，而颜色深度越高则表示函数的值越高

<img src="C:\Users\mutudeh\AppData\Roaming\Typora\typora-user-images\1570728903681.png" alt="1570728903681" style="zoom:30%;" />

再次观察下图，将离散的图像想象为连续的情况，则上述语谱图就是**从正前方的顶向下观察**

<img src="C:\Users\mutudeh\AppData\Roaming\Typora\typora-user-images\1570728423438.png" alt="1570728423438" style="zoom:27%;" />

# 倒谱 cepstrum 与梅尔系数

## 目的

提取声谱包络 (Spectrum Envelope) 以及共振峰 (Formants)，其中声谱包络即为连接共振峰的一条平滑曲线，如下图所示。

<img src="C:\Users\mutudeh\AppData\Roaming\Typora\typora-user-images\1571632011610.png" alt="1571632011610" style="zoom:40%;" />

声谱图可以认为由两部分组成

- 声谱包络
- 声谱细节

以 $X(k)$ 表示原声谱，则如下公式可以成立


$$
X(k) = \underbrace{H(k)}_{包络} * \underbrace{E(k)}_{细节} \tag{1}
$$


<img src="C:\Users\mutudeh\AppData\Roaming\Typora\typora-user-images\1571632420399.png" alt="1571632420399" style="zoom:33%;" />

为了提取包络，首先对声谱图进行 log 操作，则可以将 (1) 式中的乘法转换为加法，可写为下式


$$
logX(k) = logH(k) + logE(k)
$$


再对该 log 声谱图进行逆傅里叶变换，则将上述声谱图逆变换到`时间-振幅 ` 域中，

---

**Note:** 对 Spectrum 进行逆傅里叶变换，实际上就是在频谱上继续进行傅里叶变换。因为自身 Spectrum 就在频域中，此时又要对其进行傅里叶变换操作，则将此时 Spectrum 的横轴视为时间

---

<center>
    <img src = "C:\Users\mutudeh\AppData\Roaming\Typora\typora-user-images\1571632732966.png" width= "400"/><img src = "C:\Users\mutudeh\AppData\Roaming\Typora\typora-user-images\1571633063827.png"width="400"/></center>

根据上述的理解，对频谱进行傅里叶变换之后的横坐标为伪频率轴（实际为时间），因为之前将 Spectrum 的横轴视为时间，可以看到，因为包络的频率较低，但细节的频率很高，则经过转换之后，包络会被转换到低频域，而细节则会被转换到高频域

此时再对高频域进行切割，则可以得到包络信息在`时域` 内的表示，再对其进行傅里叶变换转换到`频域`，则可以得到包络信息

<img src="C:\Users\mutudeh\AppData\Roaming\Typora\typora-user-images\1571633862950.png" alt="1571633862950" style="zoom:33%;" />





### 梅尔频率

因为人耳为一个非线性系统，即在不同的频率上，对频率增加的感知不同，在低频内，对频率增加感知敏感（增加一点就能感知到），在高频内，对频率增加感知敏感度降低，也就对应下图，高频内三角形逐渐增大

<img src="C:\Users\mutudeh\AppData\Roaming\Typora\typora-user-images\1571634726725.png" alt="1571634726725" style="zoom:33%;" />





梅尔频率首先对经过傅里叶变换的频谱进行变换，


$$
mel(f) = 2595 * log_{10} (1+f/700)
$$
如下图所示，经过转换到梅尔频率后，其为线性增长，即对不同频率上的频率增长感知相同

<img src="C:\Users\mutudeh\AppData\Roaming\Typora\typora-user-images\1571634933453.png" alt="1571634933453" style="zoom:23%;" />



### MFCC 频率倒谱系数 Mel-Frequency Cepstral Coefficients

MFCC 唯一增加的步骤，就是对声谱 Spectrum 的处理，也就是利用


$$
logX(k) = log(Mel-Spectrum)
$$
然后对 X(k) 进行倒谱分析，提取包络，即为 MFCC 系数

<img src="C:\Users\mutudeh\AppData\Roaming\Typora\typora-user-images\1571635065628.png" alt="1571635065628" style="zoom:25%;" />