# 7. 函数

```bash
fname(){
	command
		…
	command;
}
# 使用函数时，只需输入以下命令：
# fname[ parm1 parm2 parm3 …]	
```
demo1
```bash
# param1 name
hello(){
    echo "hello $1"
}

hello "小明"
```
输出
```text
hello 小明
```