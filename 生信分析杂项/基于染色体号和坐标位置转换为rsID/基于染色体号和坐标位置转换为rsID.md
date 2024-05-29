# 基于染色体号和坐标位置转换为RSID



> 整理：Yongqiang Kong
>
> 日期：2024.5.29
>
> GitHub：https://github.com/Lonelycube



[toc]

> 参考: [https://zhuanlan.zhihu.com/p/439678589](https://zhuanlan.zhihu.com/p/439678589)
> 	  [https://zhuanlan.zhihu.com/p/410164485](https://zhuanlan.zhihu.com/p/410164485)

## 1 参考数据准备

**下载 hg19 基因组版本的 dbSNP 参考数据：** snp150_hg19.txt

- 直链下载：[https://hgdownload.soe.ucsc.edu/goldenPath/hg19/database/snp150.txt.gz](https://hgdownload.soe.ucsc.edu/goldenPath/hg19/database/snp150.txt.gz)
- 上述连接如果失效，请重新进入UCSC官网搜索并下载，UCSC官网：[https://hgdownload.soe.ucsc.edu/downloads.html](https://hgdownload.soe.ucsc.edu/downloads.html)

![1716800240939-a7c38b5d-ec8a-4d41-accc-cf8f5ad5d94e](./基于染色体号和坐标位置转换为rsID.assets/1716800240939-a7c38b5d-ec8a-4d41-accc-cf8f5ad5d94e.webp)

**下滑，找到hg19版本**
![1716800801971-8cbc94b8-61fd-46de-9011-d7862e97b220](./基于染色体号和坐标位置转换为rsID.assets/1716800801971-8cbc94b8-61fd-46de-9011-d7862e97b220.webp)
**下滑，找到snp150.txt.gz, 下载即可**
![1716800881725-4b1df604-8a0d-4623-8748-874f4297158d](./基于染色体号和坐标位置转换为rsID.assets/1716800881725-4b1df604-8a0d-4623-8748-874f4297158d.webp)

**解压并改名为 snp150_hg19.txt**



## 2 使用R语言进行ID查询转换
### 2.1 整理input文件
![image.png](./基于染色体号和坐标位置转换为rsID.assets/1716801168894-131ac70d-5412-41e3-b007-63fb9f14fd2d.webp)

﻿

### 2.2 使用R语言进行rsID转换
内存需求较大，建议使用服务器，R语言脚本为：
```r
# 确保以下两个R包已安装
# install.packages(dplyr)
library(dplyr)
library(data.table)

# 读取输入文件：snp_input.txt
tes = read.table("snp_input.txt",header=T,check.names=F,sep="\t") 
print("DONE：SNP_input")

# 读取参考文件：snp150_hg19.txt
match = data.table::fread("snp150_hg19.txt",header=T,check.names=F,sep="\t")
print("DONE：SNP_150_hg19")

# 基于参考文件提取rsID，并保存
# 如果snp150_hg19.txt文件中有对应的RS号，则比对到test.txt文件中，如果没有的话，就变为NA
need = dplyr::left_join(tes,match,by="chromosome:start") 
write.table(need, file ="clean.txt", sep ="\t", row.names =FALSE, col.names =TRUE, quote =FALSE) #保存文件

```

结果示例：
![1716803768269-be51fac7-85d9-4f81-b29e-581b5512e54d](./基于染色体号和坐标位置转换为rsID.assets/1716803768269-be51fac7-85d9-4f81-b29e-581b5512e54d.webp)

