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

### 傅里叶级数与傅里叶变换

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