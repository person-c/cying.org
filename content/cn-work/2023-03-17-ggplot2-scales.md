---
title: ggplot2-scales
date: 2023-03-17
slug: ggplot2-scales
---

## Position scales and axes

位置的scale，本质是数据到x以及y轴的映射。任何的映射都包含有limits（data-range), breaks，以及labels三个参数用于控制映射的行为。连续性变量的映射都含有一个`trans`参数可以控制变量映射的行为。

#### Numeric position scales

- limits
  
```r
p + scale_x_continuous(limits = c(1, 7))
p +  lims(x = c(1, 7), y = c(10, 45))
p + xlims()
```

有的时候你可能考虑截取x或者y某个范围内的图形，这个时候你可以这样操作：

```r
base + coord_cartesian(ylim = c(10, 35)) 
```

你可能会直接像下面这么操作：

```r
p + ylim(10, 35)
```

两者操作的区别在于，前者并没有改变原数据映射的图形，而是直接从中截取一部分图形，而后者是对超出limits的数据去除以后的重新映射。

ggplot2含有一个默认的行为，那就是对于x, 以及y轴的limits总是超出投影于x, y的数据的范围一点。这在大多数时候是有用的，不过在少部分情况下，例如绘制热图时，我们需要去掉多余的范围。对于x, y超出部分的具体大小我们可以通过加法（具体的量）或者惩罚（相对于x，y的数据范围的比例）实施。具体如下：

```r
# Additive expansion of three units on both axes
base + 
  scale_x_continuous(expand = expansion(add = 3)) + 
  scale_y_continuous(expand = expansion(add = 3))

# Multiplicative expansion of 20% on both axes
base + 
  scale_x_continuous(expand = expansion(mult = .2)) + 
  scale_y_continuous(expand = expansion(mult = .2)) 
```

实际上，你还可以精细控制x，y首端或者末端的扩展范围。

- breaks

当你手动设定breaks时，break之间的gridline会被自动去掉。修改break可以像如下操作：

```r
base + scale_x_continuous(breaks = c(1000, 2000, 4000))
```

你也可以给breaks参数传递一个函数，这个函数通过接受limits来确定返回的breasks。r包scales提供了一系列这样的函数。

除了breaks以外，你还可以设置minor_breaks，一般用于对数轴内，可以更加精细地展示某个范围内的值。如下：

```r
mb <- unique(as.numeric(1:10 %o% 10 ^ (0:3)))
base + scale_x_log10(minor_breaks = mb)
```

- labels

每一个breaks都有一个对应的lable，用于显示breaks的名称。

```r
base + 
  scale_x_continuous(
    breaks = c(2000, 4000), 
    labels = c("2k", "4k")
  ) 
```

同样地，和breaks参数一样，你也可以传递一个函数给breaks参数；该函数接收一个breaks向量，然后返回一个字符向量，作为每个breaks的label。同样地，r包scales中提供了一系列这样的函数。

```r
base + scale_y_continuous(
  labels = scales::label_dollar(prefix = "", suffix = "€")
)
```

`babels = NULL`可以帮你移去labels。

- transformations

当我们在处理连续性数据时，一般都是从数据空间到美学空间的线性映射。有时候我们可能会考虑尺度转换。大多数时候不用我们自己转换，因为有许多map函数已经提供了非线性的映射。

```r
base + scale_x_reverse()
base + scale_y_reverse()
```

实际上每个连续型变量的映射都有一个`trans`参数，用于控制映射的行为。这些转换可以通过"transformer"执行，你也可以通过函数`scales::trans_new()`构建自己的转换函数。

```r
ggplot(diamonds, aes(price, carat)) + 
  geom_bin2d() + 
  scale_x_continuous(trans = "log10") 
```

其实你完全可以自己对数据先transformation然后再绘图，两者的图形没有任何区别；唯一的区别在于自己转换数据以后绘图，label在于转换后的数据空间，而scale的label在原始数据空间。

```r
# manual transformation
ggplot(mpg, aes(log10(displ), hwy)) + 
  geom_point()

# transform using scales
ggplot(mpg, aes(displ, hwy)) + 
  geom_point() + 
  scale_x_log10()
```

>Regardless of which method you use, the transformation occurs before any statistical summaries. To transform after statistical computation use `coord_trans()`. See Section 16.1 for more details on coordinate systems, and Section 15.6 if you need to transform something other than a numeric position scale.

#### Discret position scales

ggplot2对于分类变量映射到x，或者y实，际上是将每一个分类变量映射为整数然后绘图；这对于绘图的好处在于当我们在其它地方需要使用宽度相关的参数时，我们可以将其确定为该分类变量range的某个百分比。

```r
ggplot(mpg, aes(x = hwy, y = class)) + 
  geom_jitter(width = 0, height = .25) +
  annotate("text", x = 5, y = 1:7, label = 1:7)
```

分类变量的特殊之处在于其Limits不是根据其终端点确定的，而是根据分类变量的每个可能的取值进行确定。它的breaks则没有任何区别，而他的labels则有多余的功能：它的label类似于字符向量，作为breaks的名称。
