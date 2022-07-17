---
title: 常见概率分布
author: ''
date: '2022-05-17'
slug: 常见概率分布
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

```{r}
library(ggplot2)
```

# 离散分布

**二项分布(Binomial Distribution)**：
> 多重伯努利实验中，已知事件事件 $ A $ 成功的概率为 $ p $，且实验次数 $ n $ 固定 ，那么随机变量 $ X $ —— 事件 $ A $ 发生次数 $ X $ ：
>> $$ P(X = k) = C_n^k p^k(1-p)^{n-k}, k = 0,1,...,n. $$
> 记为：$ X \sim b(n,p) $
> $ E(X) = np $ , $ D(X) = np(1-p) $

> 二项分布的特殊分布 **两点分布（Bernoulli Distribution** ，即一重伯努利实验：

**负二项分布(Negative Binomial Distribution)**：
> 多重伯努利实验中，已知事件 $ A $ 发生的概率为 $ p $，那么当事件 $A$ 第 $r $ 次发生，那么随机变量 $X$ —— 伯努利实验次数：
> $$ P(X = K) = C_{k-1}^{r-1}p^r(1-p)^{k-r}, k = r,r+1,... $$
> 记作：$ X \sim Nb(r,p) $
> $ E(X) = \frac{r}{p} $, $ D(X) = \frac{r(1-p)}{p^2} $

> 负二项分布的特殊分布  **几何分布（Geometric Distrirution)**，即当 $ r = 1 $ 时的负二项分布。
>记为：$ X \sim Ge(p) $

**超几何分布**：不放回的随机抽样，设有N件产品，其中中M件不合格品，从中不放回的随机抽取$n$件，则其中的不合格的件数服从超几何分布： 
> $$ P(X = k) = \frac{C_M^K C_{N-M}^{n-k}}{C_N^n} $$  
> 记为：$ X \sim h(n,N,M) $
> $ E(X) = n\frac{M}{N} $, $ D(X) = \frac{nM(N-M)(N-n)}{N^2(N-1)} $

**泊松分布(Possion Distribution)**：涉及到单位时间，面积，体积的计数过程，数量X:
> $$ P(X=k) = \frac{\lambda^k e^{-\lambda}}{k!}  $$ 
> 记为：$ X \sim  P(\lambda) $
> $E(X) = \lambda), D(X) = \lambda $

# 连续分布

**正态分布**
> 正态分布含有两个参数 $\mu$，$\sigma$, 其中 $\mu$ 为位置参数，控制曲线在 $x$ 轴上的位置；$\sigma$为尺度参数，用于控制曲线的参数。
> 记为：$X \sim N(\mu,\sigma)$
> $E(X) = \mu$, $D(X) = \sigma^2$

```{r}
base <-
  ggplot() +
  xlim(-4, 4)
base +
  stat_function(aes(colour = "\u03B1 = 0, \u03C3= 1"), fun = dnorm,
                args = list(mean = 0, sd = 1))
```

> 概率密度函数：
> $$ p(x) = \frac{1}{\sqrt{2 \pi}\sigma} e^{- \frac{(x - \mu)^2}   {2\sigma^2}} $$
> 分布函数：
> $$ F(x) = \int_{-\infty}^x p(t)dt = \int_{-\infty}^x \frac{1}{\sqrt{2\pi}\sigma} e^{- \frac{(t-\mu^2)}{2\sigma}}dt $$

**均匀分布**
> 记为：$X \sim U(a,b)$       
> $E(X) = \frac{a+b}{2}$，$D(X) = \frac{(b-a)^2}{12}$
> 概率密度函数：
> 分布函数：

 **指数分布**
> 记为：$X \sim Exp(\lambda)$
> $E(X) = \frac{1}{\lambda}$，$D(x) = \frac{1}{\lambda^2}$
> 概率密度函数：
> 分布函数：

**伽玛分布**
> 记为：$X \sim Ga(\alpha,\lambda)$
> $E(X) = \frac{\alpha}{\lambda}$， $D(X) = \frac{\alpha}{\lambda^2}$
> 概率密度函数：
> 分布函数

**贝塔分布**
> 记为：$X \sim Be(a,b)$
> $E(X) = \frac{a}{a+b}$， $D(x) = \frac{ab}{(a+b)^2 (a+b+1)}$
