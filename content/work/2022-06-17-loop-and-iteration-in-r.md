---
title: tidyverse中的循环与迭代
author: ''
date: '2022-06-17'
slug: tidyverse中的循环与迭代
categories: []
tags: [tidyverse]
---

## 函数式编程 -  map函数家族

许多函数需要用函数作为参数，称这样的函数为泛函(functionals)。 典型的泛函是lapply类函数。 这样的函数具有很好的通用性， 因为需要进行的操作可以输入一个函数来规定， 用输入的函数规定要进行什么样的操作。

1. `...`形参
在R函数的形参中， 允许有一个特殊的`...`形参（三个小数点）， 这在调用泛函类型的函数时起到重要作用。 在调用泛函时， 所有没有形参与之匹配的实参， 不论是带有名字还是不带有名字的， 都自动归入这个参数， 将会由泛函传递给作为其自变量的函数。 `...`参数的类型相当于一个列表， 列表元素可以部分有名部分无名， 用`list(...)`可以将其转换成列表再访问。

2. 匿名函数
 `purrr`包为在`map()`等泛函中使用无名函数提供了简化的写法， 将无名函数写成“~ 表达式”格式， 表达式就是无名函数定义， 用.表示只有一个自变量时的自变量名， 用.x和.y表示只有两个自变量时的自变量名， 用`..1`、`..2`、`..3`这样的名字表示有多个自变量时的自变量名。

3. map分类
- 变量个数 map_* map2_*,imap, pmap_*(parallel map)，
- 返回类型 *_int, *_dbl, *_chr, *_lgl, *_dfc, *dfr, modify, walk
- 判断筛选 *_if
- 多元运算 reduce

4. 示性函数
- some(.x, .p)，对数据列表或向量.x的每一个元素用.p判断， 只要至少有一个为真，结果就为真； every(.x, .p)与some类似，但需要所有元素的结果都为真结果才为真。 这些函数与any(map_lgl(.x, .p))和all(map_lgl(.x, .p))类似， 但是只要在遍历过程中能提前确定返回值就提前结束计算， 比如some只要遇到一个真值就不再继续判断， every只要遇到一个假值就不再继续判断。
- detect(.x, .p)返回数据.x的元素中第一个用.p判断为真的元素值， 而detect_index(.x, .p)返回第一个为真的下标值。
- keep(.x, .p)选取数据.x的元素中用.p判断为真的元素的子集； discard(.x, .p)返回不满足条件的元素子集。

#### 一 map函数
map函数用于向量的元素
```r
tibble(
  x = c(3, 5, 6)
) %>% 
 mutate(r = purrr::map(x, ~rnorm(.x, mean = 0, sd = 1)))
 ```

 map函数用于列表的元素
 ```r
 mtcars %>%
  group_by(cyl) %>%
  nest() %>%
  mutate(model = purrr::map(data, ~ lm(mpg ~ wt, data = .))) %>%
  mutate(result = purrr::map(model, ~ broom::tidy(.))) %>%
  unnest(result)
 ```

map函数用于数据框元素
```r
 palmerpenguins::penguins %>% 
  map_int(~ sum(is.na(.)))
```

#### map2, imap函数
map2用于2个向量元素
```
df <-
  tibble(
    a = c(1, 2, 3),
    b = c(4, 5, 6)
  )
df %>% 
  mutate(min = map2_dbl(a, b, ~min(.x, .y)))
```

 如果x有元素名， imap(x, f)相当于imap2(x, names(x), f)； 如果x没有元素名， imap(x, f)相当于map2(x, seq_along(x), f)。 iwalk()与imap()类似但不返回信息。 f是对数据每一项要调用的函数， 输入的函数的第二个自变量或者无名函数的.y自变量会获得输入数据的元素名或者元素下标。

imap同时访问元素，以及元素名

```r
iwalk(d.class, ~ cat(.y, ": ", typeof(.x), "\n"))
```

imap同时访问元素，以及元素下标
```
dl <- list(1:5, 101:103)
iwalk(dl, ~ cat("NO. ", .y, ": ", .x[[1]], "\n"))
```

#### pmap函数
```
d <- tibble::tibble(
  x = 101:103, 
  y=c("李明", "张聪", "王国"))
pmap_chr(d, function(...) paste(..., sep=":"))
```

自动匹配
```
params <- tibble::tribble(
  ~ n, ~ min, ~ max,
   1L,     0,     1,
   2L,    10,   100,
   3L,   100,  1000
)

pmap(params, ~runif(n = ..1, min = ..2, max = ..3))
pmap(params, runif)
```

#### reduce 以及accumulate函数
```
x <- list(
  c(2, 3, 1, 3, 1), 
  c(1, 5, 3, 3, 2),
  c(5, 4, 2, 5, 3),
  c(1, 4, 3, 2, 5))
intersect(intersect(intersect(x[[1]], x[[2]]), x[[3]]), x[[4]])

reduce(x, intersect)
```

```
accumulate(x, union)
```

#### walk函数
```r
plot_rnorm <- function(sd) {
  tibble(x = rnorm(n = 5000, mean = 0, sd = sd)) %>% 
    ggplot(aes(x)) +
    geom_histogram(bins = 40) +
    geom_vline(xintercept = 0, color = "blue")
}

plots <-
  c(5, 1, 9) %>% 
  map(plot_rnorm)

plots %>% 
  walk(print)
```



文中所有内容来自[数据科学中的R语言](https://bookdown.org/wangminjie/R4DS/tidyverse-beauty-of-across1.html)
## across函数

across函数第一个参数作为选择器用，按列选择，返回子数据框，然后将子数据框的每一列顺序传递给第二个参数的函数，得到每一列的结果后，合并起来返回修改后的子数据框。
across函数与summarize函数连用时，返回选择的子数据框的统计结果。
across函数mutate函数连用时，选择的子数据框经过修改后，并入原数据框。
acroos函数与其它函数连用时，一般仅作为选择器，选择子数据框。

filter函数 you if_any ,if_all与其连用。

. 的含义， 匿名函数的问题
summarize 中每一句话都是对管道符传递过来的数据框进行统计汇总生成一个数据框或者自己手动生成一个数据框，最后将每句话生成的数据框合并输出。
mutate 每一句话都是对原始数据框的列进行替换，并且保留其它不需要修改的列。

across()函数，它有三个主要的参数：

```r
across(.cols = , .fns = , .names = )
```
- 第一个参数.cols = ，选取我们要需要的若干列，选取多列的语法与`select()`的语法一致，选择方法非常丰富和人性化

   - 基本语法
      - `:`，变量在位置上是连续的，可以使用类似 `1:3` 或者` species:island`
      - `!`，变量名前加!，意思是求这个变量的补集，等价于去掉这个变量，比如`!species`
      - `&` 与 `|`，两组变量集的交集和并集，比如 `is.numeric & !year`, 就是选取数值类型变量，但不包括`year`; 再比如 `is.numeric | is.factor`就是选取数值型变量和因子型变量
      - `c()`，选取变量的组合，比如`c(a, b, x)`
   
   - 通过人性化的语句
     - `everything()`: 选取所有的变量
     - `last_col()`: 选取最后一列，也就说倒数第一列，也可以`last_col(offset = 1L)` 就是倒数第二列
     
   - 通过变量名的特征
      - `starts_with()`: 指定一组变量名的前缀，也就把选取具有这一前缀的变量，`starts_with("bill_")`
      - `ends_with()`: 指定一组变量名的后缀，也就选取具有这一后缀的变量，`ends_with("_mm")`
      - `contains()`: 指定变量名含有特定的字符串，也就是选取含有指定字符串的变量，`ends_with("length")`
      - `matches()`: 同上，字符串可以是正则表达式
   
   - 通过字符串向量
     - `all_of()`: 选取字符串向量对应的变量名，比如`all_of(c("species", "sex",    "year"))`，当然前提是，数据框中要有这些变量，否则会报错。
     - `any_of()`: 同`all_of()`，只不过数据框中没有字符串向量对应的变量，也不会报错，比如数据框中没有people这一列，代码`any_of(c("species", "sex", "year", "people"))`也正常运行，挺人性化的
   
   
   - 通过函数
     - 常见的有数据类型函数 `where(is.numeric), where(is.factor), where(is.character), where(is.date)`
    

- 第二个参数`.fns =`，我们要执行的函数（或者多个函数），函数的语法有三种形式可选：
  - A function, e.g. `mean`.
  - A purrr-style lambda, e.g. `~ mean(.x, na.rm = TRUE)`
  - A list of functions/lambdas, e.g. `list(mean = mean, n_miss = ~ sum(is.na(.x))`
  
- 第三个参数`.names =`, 如果`.fns`是单个函数就默认保留原来数据列的名称，即`"{.col}"` ；如果`.fns`是多个函数，就在数据列的列名后面跟上函数名，比如`"{.col}_{.fn}"`；当然，我们也可以简单调整列名和函数之间的顺序或者增加一个标识的字符串，比如弄成`"{.fn}_{.col}"`，`"{.col}_{.fn}_aa"`





```r
penguins %>%
  group_by(species) %>%
  summarise(
    n = n(),
    across(starts_with("bill_"), mean, na.rm = TRUE),
    Area = mean(bill_length_mm * bill_depth_mm, na.rm = TRUE),
    across(ends_with("_g"), mean, na.rm = TRUE),
  )
```

```r
penguins %>%
  drop_na() %>%
  mutate(
    across(where(is.numeric), .fns = list(log = log), .names = "{.fn}_{.col}"),
    across(where(is.character), as.factor)
  )
```

```r
penguins %>%
  group_by(species) %>%
  summarise(
    broom::tidy(lm(bill_length_mm ~ ., data = across(is.numeric)))
  )
```

```r
df      <- tibble(x = 1:3, y = 3:5, z = 5:7)
weights <- list(x = 0.2, y = 0.3, z = 0.5)

df %>%
  mutate(
    across(all_of(names(weights)),
           list(wt = ~ .x * weights[[cur_column()]]),
          .names = "{col}.{fn}"
    )
  )
  ```

```r
# 要求 x1_intercept + x1_value * x1_slope  --> x1_yhat
# 要求 x2_intercept + x2_value * x2_slope  --> x2_yhat

library(stringr)

df <- tibble(
  x1_intercept = c(0.1850, 0.1518), x2_intercept = c(0.2109, 0.3370),
  x1_value = c(0.0098, 0.0062), x2_value = c(0.0095, 0.0060),
  x1_slope = c(0.1234, 0.1241), x2_slope = c(0.1002, 0.3012),
)
df
```

```r
df %>%
  mutate(
    across(
      .cols = ends_with("_intercept"),
      .fns = ~ . + get(str_replace(cur_column(), "intercept", "value")) *
        get(str_replace(cur_column(), "intercept", "slope")),
      .names = "{.col}_yhat"
    )
  ) %>%
  rename_with( ~ str_remove(., "_intercept"), ends_with("_yhat"))
```







