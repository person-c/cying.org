---
title: ggplot2 grammar
date: 2023-11-14
slug: ggplot2 grammar
---

- [Build a plot layer by layer](#build-a-plot-layer-by-layer)
  - [Data](#data)
  - [Aesthetic mappings](#aesthetic-mappings)
  - [Geoms](#geoms)
  - [Stats](#stats)
  - [Position adjustments](#position-adjustments)
- [Scales and guides](#scales-and-guides)
  - [图例合并与分割](#图例合并与分割)
  - [Legend key glyphs](#legend-key-glyphs)
  - [Legene position](#legene-position)

# Build a plot layer by layer

ggplot2绘图首先构建一幅图，该图有着默认的数据集以及参数映射。

```r
p <- ggplot(mpg, aes(displ, hwy))
```

然后在这之上添加图层。

```r
p + geom_point()
```

每一个图层都由5个组件构成

> data: 数据来源，大多数时候每个图层的数据来源都是设置为`NULL`, 使用在`ggplot`中的数据，不过你也可以为每一个图层提供它自己的数据来源。
> mapping: 数据到美学的映射，在`aes`函数中设置
> geom: 用于绘制每个观测的几何对象，该几何对象接收额外的参数，可以通过参数`...`或者`geom_params`传递给该几何对象。
> stat: 数据的统计变换名称，用于进行一些statistical summary, 尤其是在直方图以及一些平滑拟合方面。和`geom`参数一样，统计对象也可以接收一些额外的参数，用于控制统计变换的细节。可以通过参数`...`或者`stat_params`传递给它。
> position: 提供用于调整重叠对象的方法，包括jitter, stacking or dodging.

## Data

- 如果需要在每一层指定不同的数据来源，你必须显式地写出`geom_text(data = outlier, aes(lable = model))`，因为其参数位置并不与`ggplot`函数中参数的位置一致。

## Aesthetic mappings

- 需要注意地就是映射地继承，在`ggplot`中地映射会在每一层中被继承，如果只想对某一层进行美学映射，应该在那一层单独映射。

- 另一个需要注意的就是`mapping`和`settings`的区别。`geom_point(aes(color = "red"), color = "red")`。`aes`是用于设置映射的，在里面写`color = red`只是将一个常数映射到了`color`，并不会将点的颜色设置为红色，当然你可以设置映射规则为`scale_color_identity`，这样可以实现你的想法。如果你只是想简单的改变它的颜色，在外面这样写上`color = red`即可以。将映射设置为常数，有一个用处就是生成对应的图例 - 在你想展示同样的几何对象，但是具有不同的参数时，或者加上缺失观测的图例-数据中没有该观测，但是其确实存在，你想在图例中展示这种缺失。

## Geoms

各种几何对象可以根据其需要的变量数进行划分。每一个几何对象都有它自己理解的美学参数，可以在其帮助文档中查到。

- Graphical primitives:
  - `geom_blank()`: display nothing. Most useful for adjusting axes limits using data.
  - `geom_point()`: points.
  - `geom_path()`: paths.
  - `geom_ribbon()`: ribbons, a path with vertical thickness.
  - `geom_segment()`: a line segment, specified by start and end position.
  - `geom_rect()`: rectangles.
  - `geom_polygon()`: filled polygons.
  - `geom_text()`: text.
- One variable:
  - Discrete:
    - `geom_bar()`: display distribution of discrete variable.
  - Continuous:
    - `geom_histogram()`: bin and count continuous variable, display with bars.
    - `geom_density()`: smoothed density estimate.
    - `geom_dotplot()`: stack individual points into a dot plot.
    - `geom_freqpoly()`: bin and count continuous variable, display with lines.
- Two variables:
  - Both continuous:
    - `geom_point()`: scatterplot.
    - `geom_quantile()`: smoothed quantile regression.
    - `geom_rug()`: marginal rug plots.
    - `geom_smooth()`: smoothed line of best fit.
    - `geom_text()`: text labels.
  - Show distribution:
    - `geom_bin2d()`: bin into rectangles and count.
    - `geom_density2d()`: smoothed 2d density estimate.
    - `geom_hex()`: bin into hexagons and count.
  - At least one discrete:
    - `geom_count()`: count number of point at distinct locations
    - `geom_jitter()`: randomly jitter overlapping points.
  - One continuous, one discrete:
    - `geom_bar(stat = "identity")`: a bar chart of precomputed summaries.
    - `geom_boxplot()`: boxplots.
    - `geom_violin()`: show density of values in each group.
  - One time, one continuous:
    - `geom_area()`: area plot.
    - `geom_line()`: line plot.
    - `geom_step()`: step plot.
  - Display uncertainty:
    - `geom_crossbar()`: vertical bar with center.
    - `geom_errorbar()`: error bars.
    - `geom_linerange()`: vertical line.
    - `geom_pointrange()`: vertical line with center.
  - Spatial:
    - `geom_map()`: fast version of `geom_polygon()` for map data.
- Three variables:
  - `geom_contour()`: contours.
  - `geom_tile()`: tile the plane with rectangles.
  - `geom_raster()`: fast version of `geom_tile()` for equal sized tiles.

同样的几何对象，可能有不同的定义方式，例如`geom_tile`用中心坐标`x`, `y`, 以及`width`, `height`定义矩形。`geom_rect`使用`ymax`, `xmin`, `ymin`, `xmax`定义矩形，而`geom_polygon`使用四个顶点的`x`,`y`坐标定义矩形。

## Stats

每个`geom`背后都有一个默认的统计变换，统计变换接收输入的数据框，返回一个新的含有新的计算出的变量的数据框。一些几何图形，默认的使用这些新的变量作为美学的映射。为了显式的使用这些经过统计变换计算出的新的变量，在使用的时候，使用`after_stat`包裹住这些向量。通过`stat_*`函数的文档，你可以查询到统计变换后得到的向量名称。比如下面使用比例绘制直方图，而不是数目。

```r
ggplot(diamonds, aes(price)) + 
  geom_histogram(aes(y = after_stat(density)), binwidth = 500)
```

- `stat_bin()`: `geom_bar()`, `geom_freqpoly()`, `geom_histogram()`
- `stat_bin2d()`: `geom_bin2d()`
- `stat_bindot()`: `geom_dotplot()`
- `stat_binhex()`: `geom_hex()`
- `stat_boxplot()`: `geom_boxplot()`
- `stat_contour()`: `geom_contour()`
- `stat_quantile()`: `geom_quantile()`
- `stat_smooth()`: `geom_smooth()`
- `stat_sum()`: `geom_count()`

Other stats can't be created with a `geom_` function:

- `stat_ecdf()`: compute a empirical cumulative distribution plot.
- `stat_function()`: compute y values from a function of x values.
- `stat_summary()`: summarise y values at distinct x values.
- `stat_summary2d()`, `stat_summary_hex()`: summarise binned values.
- `stat_qq()`: perform calculations for a quantile-quantile plot.
- `stat_spoke()`: convert angle and radius to position.
- `stat_unique()`: remove duplicated rows.

```r
ggplot(mpg, aes(trans, cty)) + 
  geom_point() + 
  geom_point(stat = "summary", fun = "mean", colour = "red", size = 4)
```

## Position adjustments

用于bar的位置调整

- `position_stack()`: stack overlapping bars (or areas) on top of each other.
- `position_fill()`: stack overlapping bars, scaling so the top is always at 1.
- `position_dodge()`: place overlapping bars (or boxplots) side-by-side.

用于点的位置调整

- `position_nudge()`: move points by a fixed offset.
- `position_jitter()`: add a little random noise to every position.
- `position_jitterdodge()` : dodge points within groups, then add a little random noise.

如果想要给位置对象传递参数只能通过位置对象传递

```r
ggplot(mpg, aes(displ, hwy)) + 
  geom_point(position = position_jitter(width = 0.05, height = 0.5))
```

# Scales and guides

每一种映射都对应这一种图例，对应映射图例地修改，可以通过对应地图例函数修改，对应关系如下

- continuous scales for colour/fill aesthetics `guide_colourbar`
- binned scales for colour/fill aesthetics `guide_coloursteps`
- position scales (continuous, binned and discrete) `guide_axis`
- discrete scales (except position scales) `guide_legend`
- binned scales (except position/colour/fill scales) `guide_bins`

## 图例合并与分割

对于每一层的每一个变量的美学映射，都默认生成一个图例，ggplot会尽可能地合并这些图例，以展示这些映射地变量。合并同一个变量不同美学映射前提是这些映射有同样地名称。如果某一层对该几何对象设置了settings，而你也想为该settings生成图例，可以设置`show.legend = TRUE`生成图例，当然你也可以用该方式不生成某一层地映射变量地图例，设置`show.legene = FALSE`即可。

```r
ggplot(toy, aes(up, up)) + 
  geom_point(size = 4, colour = "grey20", show.legend = TRUE) +
  geom_point(aes(colour = txt), size = 2) 
```

当多个变量映射到一个几何对象地同一个美学参数，就需要进行图例地分割，使用R包`ggnewscale`可以实现该功能

```r
base <- ggplot(mpg, aes(displ, hwy)) + 
  geom_point(aes(colour = factor(year)), size = 5) + 
  scale_colour_brewer("year", type = "qual", palette = 5) 

base
base + 
  ggnewscale::new_scale_colour() + 
  geom_point(aes(colour = cyl == 4), size = 1, fill = NA) + 
  scale_colour_manual("4 cylinder", values = c("grey60", "black"))
```

## Legend key glyphs

使用`key_glyph`参数可以改变图例中每一层地图例多边形

```r
base <- ggplot(economics, aes(date, psavert, color = "savings"))

base + geom_line()
base + geom_line(key_glyph = "timeseries")
```

## Legene position

`legend.position` 控制图例的位置，在四个角落`top`,`bottom`, `left`, `right`。

如果panel内有多余空间，你可能会想把图例放在其中，你可以给`legend.position`设定一个相对位置来实现

>Alternatively, if there’s a lot of blank space in your plot you might want to place the legend inside the plot. You can do this by setting legend.position to a numeric vector of length two. The numbers represent a relative location in the panel area: c(0, 1) is the top-left corner and c(1, 0) is the bottom-right corner. You control which corner of the legend the legend.position refers to with legend.justification, which is specified in a similar way. Unfortunately positioning the legend exactly where you want it requires a lot of trial and error.

```r
base <- ggplot(toy, aes(up, up)) + 
  geom_point(aes(colour = txt), size = 3)

base + 
  theme(
    legend.position = c(0, 1), 
    legend.justification = c(0, 1)
  )

base + 
  theme(
    legend.position = c(0.5, 0.5), 
    legend.justification = c(0.5, 0.5)
  )
```

- `legend.direction`: layout of items in legends (“horizontal” or “vertical”).

- `legend.box`: arrangement of multiple legends (“horizontal” or “vertical”).

- `legend.box.just`: justification of each legend within the overall bounding box, when there are multiple legends (“top”, “bottom”, “left”, or “right”).
