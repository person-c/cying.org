---
title: 假设检验（一）
author: ''
date: '2022-05-08'
slug: 假设检验（一）
categories: []
tags: []
---
<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  tex2jax: {
    inlineMath: [['$','$'], ['\\(','\\)']],
    processEscapes: true
  }
});
</script>

开始进行假设检验之前我们需要了解一些基本概念

> **总体**（population): A population is the entire group that you want to draw conclusions about.    
> **样本**（sample): A sample is the specific group that you will collect data from. 
> **样本统计量**：不含有未知参数的样本函数。

> 由上可知，总体是一个所有数据点的集合，而一个样本是在该集合中随机抽取一部分数据点构成的一个子集，该子集的大小即数据点的个数，即为样本大小（sample size)。因为每一次抽取都是随机的，那么每一个样本的均值以及其他参数肯定是存在差异的，那么样本的均值服从于什么分布呢？我们知道 若随机变量：
>> $$X \sim N(\mu, \sigma^2) $$

> 那么：
>> $$ \bar{X} \sim N(\mu, \frac{\sigma^2}{n}) $$   
>> 即：$$ \frac{\bar{X} - \mu}{\frac{\sigma}{\sqrt{n}}} \sim N(0,1) $$

<img src="/cn/2022-05-08-test_files/figure-html/unnamed-chunk-1-1.png" width="672" />

> 如果随机变量不服从于正态分布呢？中心极限定理给出了答案。
> 中心极限定理是关于随机变量之和的分布的定理，它给出了在什么条件下随机变量之和服从于正态分布:

>**独立同分布的中心极限定理**：设随机变量 $ X_1, X_2, ..., X_n $，相互独立，且服从于同一分布，记其期望 $ E(X_k) = \mu， D(X_k) = \sigma^2 $,那么其和的标准化变量：
>> $$ \frac{\sum_{k=1}^nX_k - n \mu}{\sqrt{n} \sigma} \sim N(0,1) $$ 

> 改写一下：
>> $$ \frac{\bar{X} - \mu}{\frac{\sigma}{\sqrt{n}}} \sim N(0,1) $$ 

>在进行t检验时，我们需要一个标准正态分布的变量，而中心极限定理告诉我们当 $ n>35 $，样本均值的分布近似于标准正态分布。 

>**Lyapunov中心极限定理**：设随机变量 $ X_1, X_2, ..., X_n $，相互独立，记随机变量 $ E(X_K) = \mu_k $ , $ D(X_k) = \sigma_k^2 $,那么：

>记：
>> $$ B_n^2 = \sum_{k=1}^{n} \sigma_k^2 $$

> 若存在正数 $ \delta $ 使得当 $ n \rightarrow{} \infty $:
>> $$ \frac{1}{B_n^{2+ \delta}} \sum_{k=1}^{n}E{{| X_k - \mu_k |}^2} \rightarrow{}  0 $$

>那么，随机变量之和的标准化变量：
>> $$ Z_n = \frac{\sum_{k=1}^{n}X_K - E(\sum_{k=1}^{n}X_K)}{\sqrt{D(\sum_{k=1}^{n}X_k)}} =  \frac{\sum_{k=1}^{n}X_k - \sum_{k=1^n \mu_k}}{B_n}  \sim N(0,1) $$




一般我们所说的各种检验比如Z检验，以及T检验主要是根据检验统计量的分布进行划分的。
# z 检验
Z检验要求总体服从于正态分布，并且总体方差已知。

# T 检验
T检验是均值的比较。

1. 单样本的均值检验（one sample t-test),用于检验样本均值与总体均值是否相等
> $ H_0 $ : 样本均值与总体均值相等(假设样本来自于该总体，且总体方差未知)    
> 计总体均值为 $ \mu $，总体方差为 $ \sigma $。
样本均值 $ \bar{X} = \frac{1}{n} \sum_{i=1}^nX_i $，样本标准差**s** =  $ \sqrt {\frac{1}{n-1} \sum_{i=1}^n(X_i- \bar{X})^2} $

> 首先构造 $ \bar{X} - \mu $，则有：
>> $$ X_i \sim N(\mu,\sigma^2) \rightarrow{} \bar{X} = \frac{1}{n} \sum_{i=1}^nX_i \sim N(\mu,\frac{\sigma^2}{n}) \rightarrow{} \frac{\bar{X}-\mu}{\frac{\sigma}{\sqrt{n}}} = \frac{\sqrt{n}(\bar{X} - \mu)}{\sigma} \sim N(0,1) $$
>由于总体方差 $ \sigma^2 $未知，我们代入一个卡方变量 $ \chi ：  
>> \frac{(n-1)s^2}{\sigma^2} \sim \chi^2(n-1) $
>两者相除，我们即消去了总体方差，从而得到检验统计量：  
$$ \frac{\sqrt{n}(\bar{X} - \mu)}{s} \sim t(n-1)  $$  
> 观察该式子可知，在零假设的情况下，我们推断出了不依赖于总体方差的样本统计量的分布，如果样本的确是来自于总体，那么该统计量显然的应该集中在0附近，如果该值偏离0太远，那么我们就认为该假设不成立。根据事先设定好的 \\(\alpha \\)，求得 \\(t_\frac{\alpha}{2} \\) 以及 \\( t_{1 - \frac{\alpha}{2}} \\),若样本统计量落在该范围之中，则说明其在0附近，零假设成立，否则拒绝零假设，选择备择假设。

> $ H_1 $：样本均值与总体均值不等。

2. 两独立样本的T检验  
> 记 $ X_1 \sim N(\mu_1,\sigma_1^2 $, $ X_2 \sim N(\mu_2,\sigma_2^2) $
> $ \bar{X_1} = \frac{1}{n_1} \sum_{i=1}^{n_1}X_{1i} $ , $ \bar{X_2} = \frac{1}{n_2} \sum_{i=1}^{n_2}X_{2i} $  
> $ s_1 = \sqrt {\frac{1}{n_1 -1} \sum_{i=1}^{n_1}(X_{1i} - \bar{X_1})^2} $, $ s_2 = \sqrt {\frac{1}{n_2 -1} \sum_{i=1}^{n_2}(X_{2i} - \bar{X_2})^2} $

2.1 总体方差相等且未知，满足 $ \frac{1}{2} < \frac{\sigma_1^2}{\sigma_2^2} < 2 $
> $ H_0 $：两样本均值相等。
> 首先构造两者的差：
>> $ \bar{X_1} - \bar{X_2} \sim N(\mu_1 -\mu_2,(\frac{1}{n_1} + \frac{1}{n_2}) \sigma^2)  \rightarrow{}  \frac{(\bar{X_1} - \bar{X_2}) - (\mu_1 - \mu_2)}{\sigma \sqrt{\frac{1}{n_1} + \frac{1}{n_2}}}  \sim N(0,1) $
> 由于两样本独立，则 $ s_1^2 $, $ s_2^2 $独立，由卡方变量的可加性：  
$$ \frac{(n_1 -1 )s^2}{\sigma_1^2}  + \frac{(n_2 - 1)s^2}{\sigma_2^2}  \sim \chi(n_1 + n_2 - 2) $$ 
> 由t分布的构造定义：
>> $$ \frac{(\bar{X_1} - \bar{X_2}) - (\mu_1 - \mu_2)}{s_p \sqrt{\frac{1}{n_1} + \frac{1}{n_2}}} \sim t(n_1 + n_2 - 1) $$
>其中 $ s_p $ 为两样本的合并标准差（pooled standard deviation), 为总体方差 $ \sigma^2 $ 的无偏估计。

2.1 若总体方差不等且未知，满足 $ s_1^2 > 2s_2^2 $,或者 $ s_2^2 > 2s_1^2 $，则采用Welch's test，其检验统计量为：
 > $$ t = \frac{\bar{X_1} - \bar{X_2}}{\sqrt{\frac{s_1^2}{n_1} + \frac{s_2^2}{n_2}}} $$  

3. 配对样本均值检验   
配对检验，针对同样的受试者，消除了其它因素的因素，仅考虑不同处理的效应。考虑每一个配对的差异，记为 $ d_i = X_{1i} - X_{2i} $ ,并设 $ n = n_1 = n_2 $ ,方差相等有 $ \sigma^2  = \sigma_1^2 = \sigma_2^2 $, 由于两样本服从于正态分布，则有：
> $$ \bar{D} = \bar{X_1} - \bar{X_2} \sim N(\mu_1 - \mu_2, \frac{2\sigma^2}{n})  \rightarrow{}  \frac{\bar{D} - (\bar{X_1} - \bar{X_2})}{\sigma \sqrt{\frac{2}{n}}} \sim N(0,1) $$ 

由t分布的构造，消去 $ \sigma $,得检验统计量：
> $$ \frac{\sqrt{n}(\bar{D} - (\mu_1 - \mu_2))}{s_d} \sim t(n - 1) $$

