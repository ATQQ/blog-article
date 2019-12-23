# 6. 文件属性与权限操作

当前目录执行    <code>ls -lrti</code>
```text
2495523 -rw-r--r--  1 root  root  65 12月 23 15:14 boot.log
```
1. i节点    :文件id     一个文件对应一个i节点   一个i节点可以对应多个文件
2. 文件类型与权限
3. 文件连接到此节点数量(不包括软链接)
4. 文件所有者
5. 文件所有组
6. 容量大小(B)
7. 最后修改时间
8. 文件名

## 链接

### 软连接
类似于win下的程序快捷方式
* 指令:ln -s 目标文件(绝对路径) 目标路径(绝对路径))
* 更改会同步到源文件
* i节点与源文件不同
* 删除源文件后,软连接失效


### 硬链接
* 指令:ln 目标文件(绝对路径) 目标路径(绝对路径))
* 更改会同步到源文件
* i节点与源文件相同
* 删除源文件后硬链接依旧可以使用(相互备份,防止源文件被误删)


## 文件类型判断
* 第一个符号
  * <code>-</code> :文件
  * <code>d</code> :目录
  * <code>l</code> :软链接
  * <code>b</code> :块设备
  * <code>c</code> :硬件设备
* 2-4字符 所属者权限
  * <code>r</code> :读权限  4
  * <code>w</code> :写权限  2
  * <code>x</code> :执行权限1
* 5-7字符 所属组权限
  * <code>r</code> :读权限  
  * <code>w</code> :写权限  
  * <code>x</code> :执行权限
* 8-10 字符 其它人的权限
  
## 修改文件权限命令 <code>chmod</code>
### 增加权限
```shell
chmod u+x,g+w,o test.txt

# 赋予所有权限
chmod 777 test.txt

# 递归赋予权限
chmod -R 777 test.txt
```

### 删除权限
```
chmod u-x,g-w,o test.txt
```

### 修改用户组权限chown
```
chown [user][:[group]] filname
```
