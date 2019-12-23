# 5.用户管理

## 分类
* 超级用户:root,拥有最高权限 UID:0
* 普通用户:有一定权限限制,可以登录系统:UID:1000-60000
* 系统用户(伪用户):不会登陆系统一般.用于维持某个服务程序UID(1-999)

## 用户相关配置文件
* 账号:/etc/passwd
* 密码:/etc/shadow

## 添加用户
* 命令:useradd
* -u:指定uid
* -d:指定用户主目录
* -g:指定用户组
* -r:指定用户是系统用户
* -s:用户登录shell的解释器
* -M:不创建主目录

## 设置密码
* 命令:passwd username


## 删除用户
* 命令:userdel username

## 添加用户组
* 命令:groupadd

## 删除用户组
* 命令:groupdel