# 1.常用命令
## 基础部分
### 切换目录 cd
* 格式:<code>cd</code> 目标目录
* 返回上一级: <code>cd</code> ../或 <code>cd</code> ..

---

### 列出当前目录的子目录与文件 ls
* 列出当前目录所有内容: <code>ls</code> 
* 详细列出当前子目录与文件:<code>ls</code> -l
* 列出的数据按照时间排序显示(降序)):<code>ls</code> -lt
* 按时间升序展示:<code>ls</code> -ltr
* 列出指定子目录:<code>ls</code> ./dir  
* 详细列出指定子目录:<code>ls</code> -l ./dir

---

### 查看当前目录 pwd
* 显示当前目录的绝对路径: <code>pwd</code> 

---

### 查看目标文件绝对路径 readlink
* readlink -f targetFilename

---
### 查看小文件内容 cat
* 查看小文件:<code>cat</code> 123.txt

---

### 查看大文件内容 more
* 查看大文件: <code>more</code> 345.txt

---

### 按行数查看文件(从前向后) head 
* 默认查看前10行:<code>head</code> filename
* 查看前n行:<code>head</code> n filename

---

### 按行数查看文件(从后向前) tail
* 默认查看后10行:<code>tail</code> filename
* 查看后n 行:<code>tail</code> n filename
* 监视动态文件变更内容:<code>tail</code> -f log.txt

---

### 文件创建 touch
* 创建一个新的空文件:<code>touch</code> empty.txt
* 同时创建多个文件: <code>touch</code> 1.txt 2.txt 

* 更新文件时间戳：<code>touch</code> -t 201811111420.55 1.txt

---

### 文件夹创建 mkdir
* 创建一级文件夹：<code>mkdir</code> test
* 创建多级文件夹：<code>mkdir</code> -p test1/test2/test3

---

### 文件夹删除　rmdir
* 删除空文件夹：<code>rmdir</code>　emptyDir 

---

### 文件删除 rm
* 删除目标文件：<code>rm</code> 123.txt
* 强制删除文件：<code>rm</code> -rf 123.txt
* 强制删除目录：<code>rm</code> -rf dir

---

### 文件拷贝　cp
* 拷贝文件到其他目录：<code>cp</code> 1.txt other
* 拷贝文件并改名称：<code>cp</code> 1.txt other/2.txt
* 拷贝一份完全一致的文件(包括旧文件的属性)：<code>cp</code> -a other/2.txt

---

### 文件移动 mv
* 移动文件：<code>mv</code> newDirectory
* 重命名文件:<code>mv</code> a.txt b.txt

---

### 对比文件差异　diff
* 对比文件差异: <code>diff</code> 1.txt 2.txt

---

### 切换主机　sh
* 切换主机：<code>ssh</code> ip
  * exit　退出

---
### 查看当前用户　id

---
### 查看系统信息　
* 查看系统信息：<code>uname</code>
* 查看系统详细信息：<code>uname</code>　-a

---
### 网络检测　ping
* 检查网络是否通畅：<code>ping</code> ip
---

### 标注输出 echo
* <code>echo</code> "hello world" 
---
### 命令文档查看　man
* 查看制定命令文档：<code>man</code> command
  *  <code>man</code> ls
  * 按 q 退出

---
<!-- ## 使用deepin 所拾得
### 解压缩 tar
    *  tar xzf filename.tar.gz -->
