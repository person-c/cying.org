---
title: "Record|记录"
slug: record
---

因为平时工作，经常遇到许多问题，所以创立一个临时记录文件，每次工作遇到的问题都记录在这里，隔一段时间再整理发布。

R中的管道符合，上一个结果居然只能传递到一个函数中，传递到嵌套函数中一直出错。

保留具体位数小数
`round(x, digits = 0)`
[rlang](https://r-lang.com/r-round/)


1
 formula is the class of the object resulting from the function '~' (tilde). You can learn more about formulas from help('~') and help('formula').

The most common way to use as.formula is to convert a string that represents the formula syntax to an object of class formula. Something like as.formula `('y ~ x')`. Also, check class`(as.formula(y~x))`.

To generate expressions from a character vector, use `parse(text=...)`.

tibble要取出一个数或者向量真是麻烦，因为它的数据结构tibbl一直不变，经常想取出一个数结果取出了一个子tibble

根据marker计算几年生存率的 [ROC曲线](https://rpubs.com/IL2/513264)

html.css学习网站 ：https://www.w3school.com.cn/h.asp
