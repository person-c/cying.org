---
title: Cluster-Example
date: 2022-12-06
slug: cluster-example
---

## 数据

数据集第一行为未注释到细菌种的占比，考虑到这一数据在样本之间较大的variation，在考虑计算样本注释的相对丰度时，纳入该数据，即每一列之和（考虑未注释到的）为1。当涉及到距离计算的时候，因为未注释到的数据并不代表任一真实的数据，所有在计算距离时剔除该行。

## 聚类

该分析采用*pam*聚类，*pam*是*k-medioids*算法实现，属于*partion-based-method*。

*partion-based-methods*算法一般会对数据进行随机选择k个（也就是你选择聚类的簇数）初始点，然后根据距离将其余的点划分给对应的点形成簇。然后根据优化函数（簇内距离和足够小，簇间距离足够大），考虑是否替换初始点，直至收敛，或者到达指定的迭代次数。

*k-medioids*算法是为了解决*k-means*方法对噪声和离群值影响比较大而提出来的算法，因为*k-means*是每次选择簇的均值作为新的中心，迭代至簇中对象分布不再变化，一个很大的极端值就会扭曲数据的分布。*k-medioids*算法则是随机选择初始簇的对象进行替换，如果目标函数——总簇内距离和，下降则选择替换上一次的中心点。但是该方法使用贪婪算法，容易陷入局部最优解。

*pam*实现过程：

1. 初始化：随机选择*n*个点中的*k*个作为中心点。
2. 计算其余的点到这几个中心点的距离，然后将其划分给其距离最近的中心点，最终将所有点划分为*k*类。
3. 随机选择非中心点对中心点进行替换，然后对点进行重新划分，如果目标函数——总的簇内距离和，增加则取消替换，减少则选择替换原中心点。
4. 重复3，直至目标函数不再减少，或到达指定迭代次数。

###### Distance metric

距离的计算方法为一种基于概率分布的方法，与*JS*散度相关。*JS*散度*（*Jensen-Shannon divergence*)，是一种正定测度，满足下面的条件：

$$
JSD(a, b) \geq 0
$$
$$
JSD(a, b) = 0, \ (only\ a = b)
$$

also symmeetric:
$$
JSD(a, b) = JSD(b, a)
$$

但是不满足三角不等式，所以不是真正的距离：
$$
JSD(a, c) \leq JSD(a, b) + JSD(b, c)
$$

但是*Jensen-Shannon divergence*的平方根是真实的距离衡量，称为*JS距离*(*Jensen-Shannon distance*),定义如下:
$$
D(a, b) = \sqrt{JSD(p_a, p_b)}
$$

在这里$P_a$和$P_b$是样本*a*,*b*的丰度的分布，*JSD(x, y)*是*x*，*y*概率分布的*JS*散度，定义如下：
$$
JSD(x, y) = \frac{1}{2}KLD(x, m) + \frac{1}{2}KLD(y, m)
$$

这里$m = \frac{x + y}{2}$, *KLD*(*Kullback-Leibler divergence*),定义如下：

$$
KLD(x, y) = \sum_{i}x_i\log\frac{x_i}{y_i}
$$

考虑到数据中有些数据为0，为了防止分子或分母为0，对这些0值用10e-6替代。

## optimal number of clusters
根据*Calinski-Harabasz(CH) index*来确定最佳距类数目。*CH index*就是之前提到的方差比准则，考虑的是簇间距离（每个簇的中心点与所有样本的中心的平方根距离）与总的簇内距离（簇中点与簇中心点的平方根距离）的比值。取值范围为$(0, +\infty]$，取值越大，说明聚类效果越好：
$$
s = \frac{B}{K - 1} \bigg/ \frac{W}{n - K} = \frac{B}{W} \cdot \frac{n - K}{k - 1}
$$

## 图形解释



