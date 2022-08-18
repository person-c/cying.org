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

tibble要取出一个数或者向量真是麻烦，因为它的数据结构tibble一直不变，经常想取出一个数结果取出了一个子tibble

根据marker计算几年生存率的 [ROC曲线](https://rpubs.com/IL2/513264)


英语写作的问题：主要的语法错误就是单复数，事态，the a
同一个主语写了两句话，后面的可以省略。
动作是用动词还是短语好？


vscode的设置，在配置文件jsonz中设置就行。


服务器端怎么编辑代码，一段一段的运行，查看结果。
服务器的图片显示问题，查看Pdf。

以及在脚本开头写了 Rscript的路径，无法直接运行R脚本


画图要达到同一个效果，可以用不同的方式进行，从一条路不行，可以从另外一条路达到同样的效果。

以 .开头的文件是用户层面的文件， 例如 .Rprofile(R启动时执行其中的代码) .Renviron (直接设置，非R代码) 。


[redhat/centos下gcc编译器版本太低无法下载R包的问题](https://shixiangwang.github.io/blog/use-new-gcc-on-centos-for-r/)

[服务器下下载Anaconda](https://developpaper.com/how-to-install-anaconda-on-a-linux-server-super-detailed/)

[一个简单且老旧的R语言实现Xgboost](https://www.hackerearth.com/practice/machine-learning/machine-learning-algorithms/beginners-tutorial-on-xgboost-parameter-tuning-r/tutorial/)

roc曲线 TPR 分母为real positive（常数），FPR，分母为real false(常数)， 改变阈值加入更多的假阳性的同时，也增加了一部分真阳性的个体。

[一个CART树的文章](https://cloud.tencent.com/developer/article/1813348)

学习真难搞啊，看不懂，不想看。

[这个ROC曲线真不错，符合我的审美，一点也不花里胡哨](https://cran.r-project.org/web/packages/plotROC/vignettes/examples.html)

真不错：
You can use lsf.str.

For instance:
list all functions in a r package
lsf.str("package:dplyr")
To list all objects in the package use ls
ls("package:dplyr")
Note that the package must be attached.
To see the list of currently loaded packages use
search()
Alternatively calling the help would also do, even if the package is not attached:

help(package = dplyr)

还有一个问题就是，已知一个函数怎么找到它所属的包呢。

git在vscode 中怎么用？
gitlen， 可以尝试一下。

原来在vscode中上传过的文件之后加入gitignor以后还是继续上传，怎么解决？

r语言的环境问题，namespace真让人头疼

