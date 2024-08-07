---
title: Base R plot element
date: 2024-02-04
slug: Base R plot element
---

使用HCL模型生成不同类型的调色板，其中chroma参数一般不变，其值越大颜色越鲜艳，则对于绘图很有帮助；其值越小，颜色越暗沉，可能在特定情况下使用；

```r
# Create a palette with 10 different hues, constant chroma and luminance
palette <- hcl(h = seq(0, 120, length.out = 5), c = 100, l = 65)

# Plot the colors
barplot(rep(1, length(palette)), col = palette, border = NA)

# Create two sequences of colors with varying luminance/ sequential palette
palette1 <- hcl(h = 0, c = 100, l = seq(90, 50, length.out = 5)) # Red to white
palette2 <- hcl(h = 240, c = 100, l = seq(50, 90, length.out = 5)) # White to blue

# Combine the two palettes to create a diverging palette
diverging_palette <- c(palette1 |> rev(), palette2 |> rev())
```

`points()` 

```r
demo("pointTypes", package = "MSG")
```

`lines()` 由连接的点定义

`xspline()` 同样由连接的点定义，不过用光滑曲线连接若干数据点。

`abline()` 由截距和斜率定义；或者水平线时的纵轴值或垂线时的横轴值。

`arrow()` 由起点坐标和终点坐标定义。

`segments()` 由起点坐标和终点坐标定义。

```r
demo("xsplineDemo", package = "MSG")
```

`rect()` 由左下角和右上角的坐标定义。

`polygon()` 线条随横纵坐标轴逐点延伸，当走到最后一点，重新延伸回第一点。缺失点将构成分界点。


`grid()` 由横纵位置上的网格数定义。


`title()` 主, 副, x, y轴标题定义。

`text()`

`mtext()` 图的四边添加文本。


`legend()`

`axis()`

[细节参考](https://bookdown.org/xiangyun/msg/tricks.html#cha:tricks)
