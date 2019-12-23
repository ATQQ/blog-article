# 2.常用基础命令2

### 清空屏幕 clear
* 格式: clear
* 清空屏幕中的所有内容
* 快捷键:ctrl+L

---
### 常看当前的用户信息 who
* 格式: who
* whoami: 我是谁

---
### 查看当前服务器的状况 uptime
* 格式: uptime
* 系统时间 开机时间 当前用户数

---
### 显示当前服务器的系统信息 w
* 格式: w
* 类似于 who 与 uptime  的结合体

---
### 查看服务器的内存使用情况 free
* 格式: free
* 查看内存使用情况
* free -m:以M为单位显示
* free -h:人性化显示单位

---
### 统计行数量 wc
* wc -l xxx.txt :统计 xxx文件的行数

---
### 查找文件中符合条件的字符串 grep
* 格式: grep 'str' xxx.txt
* 显示查找结果数量: grep 'str' xx.txt | wc -l
* 查看结果属于第几行:grep -n 'str' xx.txt
* 精确匹配:grep -w 'str' xxx.txt
* 去掉符合条件的行:grep -v 'str' xxx.txt
* 忽略大小写:grep -i 'str' xxx.txt

---
### 查找文件 find
* 格式: find dir -name filename
* 查找当前目录下的 x.txt:find ./ -name x.txt
* 过滤掉同名目录:find dir -name -type f xxx.txt

---
### 统计排好序的内容 uniq 
* 格式 : uniq filename
* 统计相同的行的数量: uniq -c filename 

---
### 对内容进行排序 sort
* 格式:sort filename
* 对统计结果排序: uniq -c filename | sort

---
### 查看磁盘使用情况 df
* 概况:df
* 带单位显示:df -h

---
### 查看网络端口使用情况 netstat
* -t:显示tcp端口
* -u:显示udp端口
* -n:致命拒绝显示的别名
* -l:指明listen的
* -p:指明显示建立相关链接的程序名

---
### 机器名 hostname

---
### 查看进程状况
* ps -ef 查看所有进程
* ps -aux 查看进程资源占用情况

---
### 关掉指定进程 kill
* kill -l 查看所有kill 信号量
* kill -9 pid :关掉指定进程

---
### 实时查看系统情况 top
* top
* q :退出

--- 
### 统计大小 du
* du filename:默认m为单位
* du -sh filename:查看文件大小详细情况
* du -sh :显示当前目录的总和

---
### echo 
* 打印:echo "hello world"
* 内容覆盖: echo '12345' > 123.txt
* 内容追加: echo '123' >> 123.txt
* 判断上一条语句是否执行成功: echo $?


### 日历查看 cal
* cal year:查看指定年份日历
* cal month year:查看指定月份的日历