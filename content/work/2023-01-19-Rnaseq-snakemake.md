---
title: RNA-seq的snakemake流程
date: 2023-01-19
slug: RNA-Seq snakemake
---

#### 序

snakemake脱胎于python，所以继承了python的语法。

#### Case in RNA seq

首先将一些信息写入yaml配置文件，用于后续流程中使用。

```yaml
samples:
    SRR12207279: data/SRR12207279.sralite
    SRR12207280: data/SRR12207280.sralite
    SRR12207283: data/SRR12207283.sralite
    SRR12207284: data/SRR12207284.sralite
    
genome: /its1/GB_BT1/cuiwei/database/mouse/mm10/mm10/genome   

gtf: /its1/GB_BT1/cuiwei/database/mouse/mm10/mm10.refGene.gtf.gz
```

- 读入配置文件
  
编辑snakefile文件。首先读入配置文件，snakemake脱胎于python，所以会将yaml信息转换为一个字典，并用变量名config指向该字典。

```python
configfile: "config.yaml"
```

- 定义最终需要输出的文件

```python
rule all:
    input:
        "result/counts/counts.txt"
```

- 将sra数据格式转换为fastq格式

涉及到将yaml配置文件中的samples用rep通配符指代，以及通过定义函数推迟通配符的识别。

```python
def get_input_sra(wildcards):
    return config["samples"][wildcards.rep]

rule extract:
    input:
        get_input_sra
    output:
        "result/raw/fq/{rep}_1.fastq",
        "result/raw/fq/{rep}_2.fastq"
    shell:
        "fasterq-dump {input} -3 -e 12 -O ./result/raw/fq/"
```

- fastq文件压缩

```python
rule pigz:
    input:
        "result/raw/fq/{rep}_1.fastq",
        "result/raw/fq/{rep}_2.fastq"
    output:
        "result/raw/fq/{rep}_1.fastq.gz",
        "result/raw/fq/{rep}_2.fastq.gz"
    shell:
        "pigz {input} -p 12"
```

- 去除序列接头
  
```python
rule cutadapt:
    input:
        "result/raw/fq/{rep}_1.fastq.gz",
        "result/raw/fq/{rep}_2.fastq.gz"
    output:
        "result/clean/fq/{rep}_1_val_1.fq.gz",
        "result/clean/fq/{rep}_2_val_2.fq.gz"
    shell:
        "trim_galore {input}  -o ./result/clean/fq/ \
        -j 4 -q 25 --phred33 --length 55 --stringency 3 --paired --gzip"
```

- 序列质检

```python
rule qc:
    input:
        "result/clean/fq/{rep}_1_val_1.fq.gz",
        "result/clean/fq/{rep}_2_val_2.fq.gz"
    output:
        "result/clean/qc/{rep}_1_val_1_fastqc.html",
        "result/clean/qc/{rep}_2_val_2_fastqc.html"
    shell:
        "fastqc {input} -t 12 -o ./result/clean/qc/"
```

- 序列比对

主要涉及到通过函数`temp`指定临时文件，在所有rule结束后sankemake会删除该文件，log文件定义，params的使用，以及指定文件的方法，`input[0], input[1]`。

```python
rule align:
    input:
        "result/clean/fq/{rep}_1_val_1.fq.gz",
        "result/clean/fq/{rep}_2_val_2.fq.gz"
    output:
        temp("result/align/sam/{rep}.sam")
    params:
        genome=config["genome"]
    threads:12
    log:
        "logs/hisat2/{rep}.log"
    shell:
        "hisat2 -x {params.genome} -S  {output} \
        -1 {input[0]}  -2 {input[1]} \
        -t -p {threads}"
```

- sam2bam并排序

主要涉及到指定需要被保护的文件，防止其被删除。

```python
rule sorted_bam:
    input:
        "result/align/sam/{rep}.sam"
    output:
        pretected("result/align/bam/{rep}_sorted.bam")
    shell:
        "samtools sort {input} -o {output} \
        -@ 12"
```

- 计数得到count值
  
主要涉及到文件扩展的写法`expand()`函数，用于多个文件整合作为输入。

```python
rule count:
    input:
        bam=expand("result/align/bam/{rep}_sorted.bam", rep=config["samples"]),
    output:
        "result/counts/counts.txt"
    params:
        gtf=config["gtf"]
    shell:
        "featureCounts {input.bam}  -a {params.gtf} \
        -o ./result/counts/counts.txt \
        -T 12  -p"
``` 

