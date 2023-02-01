---
title: "Record|记录"
slug: record
---
## 2022

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

写一下xgboost的笔记吧
[CART树](https://zhuanlan.zhihu.com/p/139523931)
[剪枝策略](https://zhuanlan.zhihu.com/p/93936294)
[XGboost](https://zhuanlan.zhihu.com/p/87885678)


写一下线性回归的笔记吧 
参考: 应用回归以及分类  
R语言教程（李东风）

[一个ggplot各种图形的代码](https://r-graph-gallery.com/index.html)
一个小的问题，因子类型的数据是基于整型数据，将其转化为数字时，它的结果会和你意料的不一样，可以考虑先将其转化为字符再转化为数字。
[突然发现youtube上有个advanced R一系列的视频](https://www.youtube.com/watch?v=u2cI3oGgW7E&list=PL3x6DOfs2NGi9lH7q-phZlPrl6HKXYDbn&index=24)

随机种子的问题，每一次的随机好像都需要设置随机种子。

R中的字符处理问题！！！

把fastdummy 生成的表格转化为 tibble 时，其中的结果发生了变化。

[关于ROC计算的一个动态图片](https://i.stack.imgur.com/YFosL.gif)感觉很简单直接。

[一个简单的r教程](https://swcarpentry.github.io/r-novice-inflammation/)

auc与准确率评价指标，其实两者并没有什么关系。

[关于on.exit()函数](https://coolbutuseless.github.io/2021/03/12/on-on.exit/)

[ggplot2中如何组合多个图形](http://www.sthda.com/english/wiki/wiki.php?id_contents=7930)
[Assign intermediate output to temp variable as part of dplyr pipeline](https://stackoverflow.com/questions/40369832/assign-intermediate-output-to-temp-variable-as-part-of-dplyr-pipeline)

rna seq怎么搜索：rna-seq analysis pipeline tutorial

/boot：存放的启动Linux 时使用的内核文件，包括连接文件以及镜像文件。
/etc：存放所有的系统需要的配置文件和子目录列表，更改目录下的文件可能会导致系统不能启动。
/lib：存放基本代码库（比如c++库），其作用类似于Windows里的DLL文件。几乎所有的应用程序都需要用到这些共享库。
/sys： 这是linux2.6内核的一个很大的变化。该目录下安装了2.6内核中新出现的一个文件系统 sysfs 。sysfs文件系统集成了下面3种文件系统的信息：针对进程信息的proc文件系统、针对设备的devfs文件系统以及针对伪终端的devpts文件系统。该文件系统是内核设备树的一个直观反映。当一个内核对象被创建的时候，对应的文件和目录也在内核对象子系统中
指令集合：

/bin：存放着最常用的程序和指令
/sbin：只有系统管理员能使用的程序和指令。
外部文件管理：

/dev ：Device(设备)的缩写, 存放的是Linux的外部设备。注意：在Linux中访问设备和访问文件的方式是相同的。
/media：类windows的其他设备，例如U盘、光驱等等，识别后linux会把设备放到这个目录下。
/mnt：临时挂载别的文件系统的，我们可以将光驱挂载在/mnt/上，然后进入该目录就可以查看光驱里的内容了。
临时文件：

/run：是一个临时文件系统，存储系统启动以来的信息。当系统重启时，这个目录下的文件应该被删掉或清除。如果你的系统上有 /var/run 目录，应该让它指向 run。
/lost+found：一般情况下为空的，系统非法关机后，这里就存放一些文件。
/tmp：这个目录是用来存放一些临时文件的。
账户：

/root：系统管理员的用户主目录。
/home：用户的主目录，以用户的账号命名的。
/usr：用户的很多应用程序和文件都放在这个目录下，类似于windows下的program files目录。
/usr/bin：系统用户使用的应用程序与指令。
/usr/sbin：超级用户使用的比较高级的管理程序和系统守护程序。
/usr/src：内核源代码默认的放置目录。
运行过程中要用：

/var：存放经常修改的数据，比如程序运行的日志文件（/var/log 目录下）。
/proc：管理内存空间！虚拟的目录，是系统内存的映射，我们可以直接访问这个目录来，获取系统信息。这个目录的内容不在硬盘上而是在内存里，我们也可以直接修改里面的某些文件来做修改。
扩展用的：

/opt：默认是空的，我们安装额外软件可以放在这个里面。
/srv：存放服务启动后需要提取的数据（不用服务器就是空）

[vscode server failed to start server](https://stackoverflow.com/questions/67976875/vs-code-remote-ssh-the-vscode-server-failed-to-start-ssh)
[vscode 写入的管道不存在](https://blog.csdn.net/kaixinjiuxing666/article/details/121452240)
[conda的安装与使用](http://t.zoukankan.com/qiu-hua-p-14617317.html)
conda中怎么下载软件，直接去这个[网站](https://anaconda.org/)搜索就可以了。

[sra怎么下载数据](https://www.reneshbedre.com/blog/ncbi_sra_toolkit.html)

[mm10 refgtf](http://hgdownload.soe.ucsc.edu/goldenPath/mm10/bigZips/genes/mm10.refGene.gtf.gz)
hisat2官网已经提供了建好的索引，可以直接下载。

[local BLAST error: BLAST Database error: Error: Not a valid version 4 database](https://bioinformatics.stackexchange.com/questions/13252/local-blast-error-blast-database-error-error-not-a-valid-version-4-database)
数据库与版本的Blast问题，很多生物信息问题报错都是版本的问题？

[blast教程](https://conmeehan.github.io/blast+tutorial.html)

R多个变量的绘图以及保存:
https://aosmith.rbind.io/2018/08/20/automating-exploratory-plots/
https://cmdlinetips.com/2022/06/create-a-list-of-ggplot-objects-and-save-them-as-files/
https://stackoverflow.com/questions/4856849/looping-over-variables-in-ggplot
[这个各种技巧的网站不错](https://cmdlinetips.com/)

[Rmarkdown中文指南](https://cosname.github.io/rmarkdown-guide/)
```yaml
---
title: "Untitled"
output:
  bookdown::pdf_document2:
    latex_engine: xelatex
header-includes: 
  - \usepackage{ctex}
date: "2022-10-22"
---
```

软件的使用小技巧：直接去官网查文档或者手册；google 搜索xx tutuorial

计算机集群的使用？
[一个集群的使用技巧](http://star.mit.edu/cluster/docs/0.93.3/guides/sge.ht)

[改变tile的矩形大小的技巧](https://stackoverflow.com/questions/23897175/adjust-ggplot2-geom-tile-height-and-width)

[Remove multiple sequences from fasta file](https://stackoverflow.com/questions/55636069/remove-multiple-sequences-from-fasta-file)
[rmarkdown, 你以为它是png文件，实际上它不是](https://stackoverflow.com/questions/61721837/cannot-use-include-graphics-to-insert-png-in-rmarkdown-error-file-is-not-in-pn)

突然发现在用管道的时候，函数如果没有参数，可以不带括号??

[sPLS-DA](http://mixomics.org/case-studies/splsda-srbct-case-study/)
[rglx11显示问题]


[完整详细的PCA教程](https://enterotype.embl.de/enterotypes.html#clustering)

[vscode找不到.vsc.*函数的问题](https://github.com/REditorSupport/vscode-R/issues/1094)

嵌套函数一般会涉及到环境的层级问题，locked bind的问题？

[r包开发](https://cosx.org/2011/05/write-r-packages-like-a-ninja/)
[r包开发](https://r-packages-zh-cn.readthedocs.io/zh_CN/latest/%E7%AC%AC%E4%BA%8C%E7%AB%A0%20%E6%95%B4%E4%B8%AA%E6%B5%81%E7%A8%8B.html)

[Latex math cheet sheet](https://joshua.smcvt.edu/undergradmath)

ggplot2画图步骤：、
考虑图形元素
考虑映射scale
考虑guide information, guides
考虑主题。

编程语言都可以调用系统内置的命令？[r通过`system`函数](https://www.r-bloggers.com/2021/09/how-to-use-system-commands-in-your-r-script-or-package/)，python通过os模块？这个东西叫接口吗，是否还可以调用其它任何语言或者软件的借口？

## 2023