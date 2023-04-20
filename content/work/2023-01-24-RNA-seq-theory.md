---
title: RNASeq theory
date: 2023-01-24
slug: RNASeq thory
---

<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  tex2jax: {
    inlineMath: [['$','$'], ['\\(','\\)']],
    processEscapes: true
  }
});
</script>

- [序-生物背景](#序-生物背景)
- [实验篇](#实验篇)
    - [RNA 测序文库](#rna-测序文库)
    - [测序技术](#测序技术)
    - [实验数据质控](#实验数据质控)
- [RNASeq上游分析篇](#rnaseq上游分析篇)
    - [Quality Control](#quality-control)
    - [mapping](#mapping)
    - [RNA seq 定量](#rna-seq-定量)
        - [通过参考基因组进行定量](#通过参考基因组进行定量)
    - [RSEM转录水平定量](#rsem转录水平定量)
- [RNASeq下游分析篇](#rnaseq下游分析篇)
    - [差异分析](#差异分析)
- [基因注释与富集分析](#基因注释与富集分析)
    - [多样本数据分析](#多样本数据分析)


## 序-生物背景

组学分析包括基因组学(全外显子组学，全基因组学)，转录组学（有参，无参），表观组学（chip-seq，甲基化等）。

RNA-seq的重要在于，很多情况下疾病的原因是蛋白的原因，而蛋白是由mRNA翻译而来，通过RNA我们可以了解到究竟是哪些蛋白发生发生了变化，或者转录出现了问题。对于组学分析而言，你需要知道哪些物种是已经被测序的，对于已测序的动物，你可以在Ensembl这个网站找到，植物在Ensembl Plant查询。

基因组包括细胞核基因组以及细胞质基因组（主要是线粒体），人基因组大概3.1Gbp，其中coding- area 1.1%，RNA genes以及 regulatory area 3%。线粒体16.6Kb。

基因在染色上分布不均匀。人类大概有20000蛋白基因，7000RNA基因，6号染色体基因数目最多。

rRNA 18 / 28 5.8 5 - 40/ 60。可以从NCBI下载到rRNA序列信息。

tRNA mapping时的参考序列需要加CCA, 组氨酸的参考序列+ G。有的tRNA含有内含子。可以从GtRNAdb下载tRNA序列

mRNA 序列信息可以从Uniprot下载。

microRNA 可以从miRbase中下载。

数量：rRNA(80-90%) tRNA(10%)，mRNA(1-5%)，直接测序大部分都是tRNA，提取mRNA(通过其poly A)，或者去除tRNA再测序。

## 实验篇

#### RNA 测序文库

mRNA 建库

1. mRNA or total RNA
2. remove contaminant DNA (remove rRNA or select mRNA?)
3. fragment RNA
4. reverse transcrible into cDNA（strand specefic?)
5. ligate sequence adaptors

建库时的几个问题。

- 是去除tRNA还是特异性保留mRNA？

  poly A+ RNA-seq方法通过与mRNA的poly A尾巴特异性结合选择mRNA(无法区分正负链，基因overlap ，正负链都可能是基因)，但是组蛋白RNA没有poly A尾巴。

  rRNA - RNA-seq方法通过酶降解tRNA。

  两种建库方式的原始数据还是有差异的。

- 如何保留链特异性？

  通过dUTP method，加入dUTP合成，切割该链条。

#### 测序技术

illumina测序原理可以参考这里。

reads1/reads2（/ - 互补） + adapater = fragment  
reads1/reads)（/ - 互补) = insert distance

- illumina测不长的原因？
  
  每测一轮都有可能有错误（同一簇不同步），后面越来越差，杂色参杂。

- 片段为什么要均一？  
  
  短片段更容易在flow cell 结合，导致测出来的都是短片段

- 为什么需要保持碱基平衡？

  150次flow cell快照，每簇都同一色不好解方程。

- adapter序列出现在结果中？
  insert短，没有150bp（只会出现在3端），fasta从左到右边，只有右边有接头；测序是从5端到3端进行测序。

#### 实验数据质控
  
total RNA提取的质控标准：RIN(RNA Integrity Number)，根据5S, 18, 28S的峰值进行评估，范围0-10。（6-6.5，7）

## RNASeq上游分析篇

#### Quality Control

质控主要需要考虑一下几个方面：

- 去除adapter
- 去除低质量reads
- 去除reads部分低质量区域
- 为下一步分析做准备；例如研究可变剪切，需要把reads修剪成等长。

关于fasta文件：

第一行，@reads名字（测序仪编号:lane:tail:x:y)  
第二行，序列  
第三行，+（reads名字/没有）  
第四行，phred value + 33 对应的ASSCI编码  
  $Phred("F") = 70 - 33 = 37$(Sanger 标准)  
  $Phred = -10 * log_{10}(error\ probability)$

  Phred40 = 0.0001 error rate  
  Phred30 = 0.001 error rate  
  Phred20 = 0.01 error rate  
  Phred10 = 0.1 error rate  

质检报告html图表:

per base quality：总reads每个位置碱基的犯错概率（下限q30）。  
per tile sequence quality：如果有花的，那么整个tile可能都有问题。  
GCcontent：不同物种GC含量不一样，样品可能被污染。  
per base N content: 杂色，未测出碱基是什么。  
sequence duplicate level：重复reads展示。  
adapter content：序列的接头检测  

质控参数：

quality -10

1. Phred - 10
2. 数据从右向左累加
3. 在累加的最小值处进行切割，保留前面的。

- 概览
  
FASTQ(QC, mapping-找reads在基因组的位置)- SAM（sequence alignment)- BAM（压缩的sam文件）

bam-质控（比对情况），计数，标准化，找差异表达

改进：翻译效率的问题，降解未结合核糖体的mRNA区域。
RNA结合蛋白鉴定：fastq-bam-chip-seq
待写。

#### mapping

- alignment（低通量，两条/多序列）
  pairwise multiple

  pairwise: 全局，局部（needleman-wunch, smith-waterman）

  blast: one vs many
  序列切短，相似再扩展。

- mapping（reads 回溯基因组）
  many vs one(bwt算法-bwa bowtie)

  1. mapping回成熟的mRNA参考序列，不用处理可变剪切，不能发现新的转录本。
  2. mapping到参考基因组，可以发现新的转录本，进行isoform层次的定量，但是不能使用之前的DNA mapping软件。
   
mapping的可变剪切问题：

  1. exon-first approach(pseudogene-mRNA逆转录cDNA插入基因组，贴到假基因区)
  2. seed-extend approch
  3. potential limitations of exon-first approches。

参考基因组(序列）下载：  

UCSC genome broswer/download/human/sequence data by chromsome

合并染色体序列信息：  
chr2.fa.gz
`cat chr1.fa.gz chr2.fa.gz > ref_hg38.fa`

\>chr1  
NNNNNNNNNN(端粒的占位符)  
ATCGGGGGGGG  
\>chr2  
NNNNNNNNNN  
ATCGGGGGGGG

Ensembl: ensembl species list/human/gene assemble/download DNA sequence  
NCBI: refseq/  

参考基因注释文件（GTF,GFF-序列哪些位置是基因等)：  
ussc/tools/table browser

注释文件每列含义：待写

Note：注释文件和参考基因组match

**BWT(burrows wheeler transform)**

假定：ref- ACAACG  
ACAACG& 循环  
1. &ACAACG  
   G&ACAAC  
   CG&ACAA  
   ACG&ACA  
   AACG&AC  
   CAACG&A
2. 得到的字符矩阵根据第一列首字母排序（&ACGT)，得到排序后的字符矩阵。
3. index就是排序的字符矩阵的最后一列

排序后的字符矩阵有下面两个性质：

- 每一行的第一个字符和最后一个字符一定在原始字符串的相连；且后一个字符在序列中在第一字符前。
- 第一列和最后一列字母（例如同一个A..)的相对次序不变；也就是第一列中第一个A和最后一列的第一个A实际上是序列中的同一个A。

Index在mapping中的作用如下：

1. 根据index还原排序后的字符矩阵矩阵的第一列
2. 根据上面两条性质反推出reference。
3. 根据排序矩阵第一列和最后一列比对，从序列后面，矩阵第一列开始搜索

**后缀树算法- suffix tree**

1. 同bwt得到未排序的字符矩阵。
2. 按第一列首字符排序得到排序的字符矩阵。
3. 构建suffix tree（存储树信息以及相对位置 信息，index索引非常大）
4. 从树的根节点出发。

实际上后缀树存储了序列某个字符后的所有可能性，例如某一字符为A，后缀树就穷举了序列中A后面所有字符的可能并保存下来，这种做法就是用空间换时间。

一些mapping软件：

tophat/tophat2
构建索引：
`bowtie2-build --threds 6 ref_hg38.fa ref_hg38.fa`

star-基于后缀树

1. reads切成小的seed，找到seed位置
2. 符合的seed拼一起

hisat2(tophat的升级版) - 全基因组Index，切割为55000份建立index。先定位在哪个小的index，然后在短的index搜索。（两层的bwt结构；其优点在于考虑了SNP信息（mapping时可以替换）

构建hisat2 index：
SNP信息：dbSNP commom（ucsc genome/annotation/sql/common.txt.gz)  
可变剪切信息：GTF  
参考基因组信息：Genome FASTA  

sam flag信息

`samtool sort`  PG bam（哪些操作）

bam 文件建立索引方便查看mapping信息。 `samtool index`，任意一段
`samtool view -h`


#### RNA seq 定量
  
###### 通过参考基因组进行定量

**样本内标准方法**

除以测序深度（reads数目），基因长度（bp)，进行单位化，分子乘以$10^6$（每百万reads)，
乘以$10^3$(基因长度1000bp)，1单位rpkm含义就是每百万reads中长度为1kb的基因的reads数目为1。

r-reads, f-fragments, p-per, k-kilobase, m-million。

RPM or CPM：仅对测序深度进行单位化。
$$
\frac{Number\ of\ reads\ mapped\ to\ gene * 10^6}{Total\ number\ of\ mapped\ reads}
$$
RPKM, FPKM：同时测序深度以及基因长度进行单位化。对于双端测序，fragment就是同一对reads。单端测序FPKM等于RPKM。
$$
\frac{Number\ of\ reads\ mapped\ to\ gene * 10^6 * 10^3}{Total\ number\ of\ mapped\ reads * gene\ length\ in\ bp}
$$

TPM:  
TPM假设每个样本的基因数值加和相同，其简单的为样本内基因Fpkm 的百分数*$10^6$。

**样本间标准化**

直接计算比例：
$$
C_j = \frac{10^6}{D_j}
$$
其中$D_j$为样本$j$测序深度，$C_j$为样本$j$校正系数。这种方法容易受到极端值的影响。

quantile：
$$
C_j = \frac{exp\left( \frac{1}{N}\sum_{l = 1}^{N}log(D_{l}Q_{l}^{(p)})\right)}{D_{j}Q_{j}^{(p)}}
$$

样本的分位数均值与某一个样本的分位数比值。其中$D_j$为样本$j$的测序深度，$Q_j^{(p)}$为样本$j$的$p$分位数，$C_j$为样本$j$的校正系数。

RLE(relative log expression) - cufdiff；Deseq2默认方法：

假定行为基因，列为样本。

1. 每行的几何平均数。
2. 每行除以该行的几何平均数，得到新矩阵。
3. 新矩阵每列的中位数就是该列样本的校正因子。

$$
C_j = median_g\left( \frac{K_{gj}}{(\prod_{l=1}^{N}K_{gl})^{\frac{1}{N}}} \right)
$$

TMM(edge R) Trimmed mean of M-values：

假设前提：total reads受高表达基因影响。大多数基因的表达量不变。

RNA-seq的定量在MA plot反应为，1）大多数点应该贴近于M = 0该直线附近。2）高表达基因的应该贴近于M = 0该直线附近———横轴是按基因的几何平均由低（左）到高（右边）排布的。

MA plot：
X-A：两组样本的几何平均数
$$
A = \frac{1}{2}log_2{(RG)}
$$
M-Y：两组样本的fold change。
$$
M = log_2{(R/G)}
$$

变化基因以及高表达的基因的阈值可选。

#### RSEM转录水平定量

reads直接mapping到mRNA上，解决可变剪切的问题。reads来自于哪个isoform?从极大似然估计到EM算法，可以得出每个isoform的count数目。

## RNASeq下游分析篇

#### 差异分析

基因的定量是一个抽样的结果RNA-cDNA-RNA

RNA-Seq的前提：

- 绝大多数基因的表达量不变
  
- 高表达基因的表达量不变

- 如果需要绝对定量，使用提前绝对定量的内参（spike-ins)，ERCC countrol。

对于cell line进行差异分析，需要2-3个repeat，对于生物体进行差异分析需要3-个repeat，对于群体而言，需要成百上千个repeate。

- p-value校正：

p排序$\rightarrow$ 新p = p* 检验次数m $\rightarrow$保持原有顺序（第一次排序的结果）调整顺序

BH校正 ：p排序$\rightarrow$新p = p * 检验次数 / 序号$\rightarrow$保持原有顺序调整p值。

- RNA-Seq定量的分布模型

**RNA-Seq定量的二项分布**：考虑一个基因，其它所有基因为一部分。那么基因A的出现次数二项分布。

**RNA-Seq定量的泊松分布**：因为一个基因出现的概率很低，p很低，二项分布接近于泊松分布。

**RNA-Seq定量的负二项分布**：

实际上RNA-seq的数据并不服从泊松分布，泊松分布期望与方差相等，然而RNA-Seq图像是over-dispersion的。随均值增加，方差也变大。这种现象称为short noise。

泊松分布：
$$
E(K_g) = Var(K_g) = \lambda
$$

RNA-seq short noise现像通过对泊松分布的修正描述。

$$
E(K_g) = \lambda, \ Var(K_g) = \lambda + \phi\lambda^2
$$

负二项分布具有此特性。所有RNA-seq基因的分布修正为负二项分布。

差异检验的零假设：
$$
\lambda_{g}A = \lambda_{g}B
$$

问题变成了估计$\lambda_g$，$\phi_g$，这个过程叫做estimate dispersion，不同软件的估算方法不同。

以前的统计检验思路，列联表卡方检验或fisher检验。基因同分布检验，fisher对大数字敏感，对小数字不敏感。

- RNA-seq的绝对定量

- ERCC 建库加入定量的已知ERCC spike-in mRNA
  
- house kepping(3000-4000）基因定量
  输入文件是 未经过校正的count数据，认为不变的基因list(spike in)
  输出就是校正过的数据。

## 基因注释与富集分析

检验：GO kegg注释就是列联表的卡方检验。  
输入：一个感兴趣基因集，基因注释信息。  
输出：基因基是否在某一类注释信息中。  
需要认为设定感兴趣基因集的阈值。  

GESA：解决人为设定基因集的问题。
输入：全部的基因变化信息，一个感兴趣的通路的基因集合（GESA msi)。  
输出：是否与整个感兴趣的通路相关。  

图片认知（待写）

非模式生物的富集分析：有参，无参。  
annotationhub。  

#### 多样本数据分析

TCGA是肿瘤数据库，可以通过firebrowser下载其中的数据。GTex是正常人的数据。

关于pearson相关系数以及spearman相关系数：spearman相关系数仅仅是对数据的排序序号进行pearson相关分析的结果。

WGCNA：只是简单对基因进行聚类，通过最终聚类效果的评价指标选取距离计算的指数。

多样本差异表达基因：

将负二项模型通过对数连接函数写作广义线性模型，并对其参数进行似然比检验：

$$
log\mu_{gi} = x_{i}^{t}\beta_g + logN_i
$$

其中$log\mu_{gi}$是基因$g$在样本$i$的观测值，$x_{i}^{t}$设计矩阵，$N_i$是样本的测序深度。差异检验就是对$\beta_g$是否全为0的检验。