---
title: easybio
subtitle: To make common bioinformatic analysis easier
date: '2023-05-31'
---

## Overview

This is a simple package to make my work easier, with some encapsulated functions to achieve daily common analysis with a uniform interface.

## `analysis` function

All common analyses can be implemented by function `analysis`, by specifying the `object` and assigning `task` to the `object`. A concrete example is as follows:

```r
library(org.Hs.eg.db)
data(gene_vector)
y <- analysis(object = gene_vector, task = 'go')
```

Here it will automatically call the corresponding function to achieve analysis using some default arguments. The analysis's details can be adjusted by extra arguments if you customize some of the arguments. For example, you can customize the `ont` argument to analyze all GO categories analysis:

```r
y <- analysis(object = gene_vector, task = 'go', ont = 'ALL')
```

If you want to do KEGG analysis, just assign `task` to kegg.

```r
data(kegg)
y <- analysis(object = kegg, task = 'kegg')
```

## `plot` function

All analyses' results can be visualized using `plot` function which extends the `plot` generic in the `base` package. you can easily visualize the results just like this:

```r
p <- plot(y)
```

The `plot` function returns a `ggplot2` object which is easy to adjust the details of the figures according to the user's requirements.

```r
library(viridis)
p <- p + scale_fill_viridis(discrete = true)
```

## Object Reductor

Reductor is an `R6` object which is mainly used to view the results of dimensional reduction in different arguments combinations.

```r
library(palmerpenguins)

x <-
  penguins |>
  tidyr::drop_na()

y <- x$species
x <- x |>
  dplyr::select(where(is.numeric)) |>
  dplyr::select(-year)
x <- scale(x)
# show the results of different arguments
happy <- Reductor$new("tsne")
set.seed(20230530)
tune_fit <- happy$tune(x,
  perplexity = c(30, 40, 50, 60),
  n_iter = c(1000, 2000, 2500)
)
happy$plot(tune_fit, y)
```
