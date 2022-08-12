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

html.css学习网站 ：https://www.w3school.com.cn/h.asp

英语写作的问题：主要的语法错误就是单复数，事态，the a
同一个主语写了两句话，后面的可以省略。
动作是用动词还是短语好？


vscode的设置，在配置文件jsonz中设置就行。


服务器端怎么编辑代码，一段一段的运行，查看结果。
服务器的图片显示问题，查看Pdf。

以及在脚本开头写了 Rscript的路径，无法直接运行R脚本

[linux下怎么写r脚本](http://www.cureffi.org/2014/01/15/running-r-batch-mode-linux/)

自己目录下的R 和 全局下的R使用的是哪一个，有必要自己下载一个R,如果全局已经有了。


环境变量的问题。 
[原文链接](https://medium.com/chingu/an-introduction-to-environment-variables-and-how-to-use-them-f602f66d15fa)
一个预先定义的值 key = value, 应用或环境在启动时从这里获取变量值，指定应用在执行时其需要的值。
全局环境变量， 都可以获得的变量。 局部环境变量，某些可以获得这些值。
在写代码时将一些敏感的变量值，独立出来作为预先定义的环境变量文件.env，并将其放入.gitignore，然后将代码上传到gitHub，以防止一些隐私信息泄露。

不同操作系统对环境变量的管理方式不同。

环境变量的设置（Linux下）

[原文链接](https://www.serverlab.ca/tutorials/linux/administration-linux/how-to-set-environment-variables-in-linux/)
设置
export NAME=VALUE
查看 echo $NAME

删除环境变量： unset NAME

listing all set environment variables: set


Persisting Environment Variables for a User: 
把export 命令写入 user's profile script,这个脚本在每次用户登录时执行。

1. open the current user's profile into a text editor
`vi ~/.bash_profile`
2. Add the export command for every environment variable you want to persist
`export JAVA_HOME=/opt/openjdk11`
3. save your changes.
4. `source ~/.bash_profile` 立即执行该bash.profile 导入环境变量。

export environment variable 
view all export variable: export 
Setting Permanent Global Environment Variables for All Users
1. Create a new file under /etc/profile.d to store the global environment variable(s). The name of the should be contextual so others may understand its purpose. For demonstrations, we will create a permanent environment variable for HTTP_PROXY.

`sudo touch /etc/profile.d/http_proxy.sh`

2. Open the default profile into a text editor.

`sudo vi /etc/profile.d/http_proxy.sh`

3. Add new lines to export the environment variables
`export HTTP_PROXY=http://my.proxy:8080`


vscode r环境设置：设置了json参数配置，需要重启以后，参数才能生效。

其它目录下已经有了需要的软件，自己目录下还需要下载该软件吗。
自己的Home目录下设置了环境变量，在其它目录下可以使用吗。

画图要达到同一个效果，可以用不同的方式进行，从一条路不行，可以从另外一条路达到同样的效果。

[R的包路径设置问题](https://www.r-bloggers.com/2020/10/customizing-your-package-library-location/)

以 .开头的文件是用户层面的文件， 例如 .Rprofile(R启动时执行其中的代码) .Renviron (直接设置，非R代码) 。


[redhat/centos下gcc编译器版本太低无法下载R包的问题](https://shixiangwang.github.io/blog/use-new-gcc-on-centos-for-r/)

[服务器下下载Anaconda](https://developpaper.com/how-to-install-anaconda-on-a-linux-server-super-detailed/)

[一个简单且老旧的R语言实现Xgboost](https://www.hackerearth.com/practice/machine-learning/machine-learning-algorithms/beginners-tutorial-on-xgboost-parameter-tuning-r/tutorial/)