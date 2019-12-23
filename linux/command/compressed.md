# 7. 文件归档与解压缩

## 文件归档
一个或者多个文件的集合

## 解压缩命令 gzip;xz
* gzip:压缩速度快,压缩比例低
  * 扩展名:.gz
  * 不能压缩目录
  * 不保留源文件:gzip 1.txt
  * 保留源文件:gzip -c 1.txt > 1.txt.gz

* gunzip:解压.gz文件
  * 不保留源文件:gunzip 1.txt.gz
  * 保留源文件:gunzip -c 1.txt.gz > 1.txt
---
* xz
  * 可以压缩目录
  * 与gzip用法一致
* unxz
  * 保留源文件解压:unxz -d -k 123.txt.xz


## 归档与压缩命令tar
* -c :创建新文件(必须)
* -f :指定文件格式(必须)
* -v :显示详细过程
* 归档指定一个或多个目录:tar -vcf name.tar dir1 dir2
* 列举归档文件:tar -tvf name.tar
* 解开归档文件:tar -xf name.tar
* 归档并压缩:tar -zvcf name.tar.gz dir1
* 解压解档:tar -xf name.tar
* 解档到指定目录:tar -xf name.tar.gz -C 指定的解压路径