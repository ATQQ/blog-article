# 4. vi编辑器使用

* vi    进入vi编辑器
* i     进入插入模式
* o     换行进入插入模式
* esc   进入命令模式
* :     进入底行模式(需先进入命令模式))

## 命令行模式使用
* $ 调到行尾
* gg 调到行首
* G 调到行尾
* x 删除一个字符
* dd 删除一行
* u 复原操作类似于 ctrl+z
* v 选中范围 按y复制
* yy 复制一行* 
* p 粘贴复制的内容

## 底行模式
* set nu        显示行号
* set nonu      关闭行号显示
* number        跳转到第number行
* /context      跳转到指定的context的行
* %s/待替换的内容/替换结果/g    替换指定的字符串
* n1,n2s/待替换的内容/替换结果/g    替换n1,n2之间指定的字符串
* q!            强制退出
* wq            保存退出
* q             退出
* !command      暂时离开vi编辑界面执行指定的command
* !ls /home     展开home目录下的内容