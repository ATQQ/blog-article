# shell与crontab定时器的结合

## crond服务
>以守护进程方式在无需人工干预的情况下来处理一些列的作业指令与服务

* 查看服务状态
  * systemctl status cron.service
* 停止服务
  * systemctl stop cron.service
* 启动服务
  * systemctl start cron.service
* 重启服务
  * systemctl restart cron.service

## crontab
* 指令格式:crontab [options]
* -l:列出当前存在的crontab
* -e:编辑crontab
* -r:删除所有的任务
* 内容格式:
  ```
  *  *  *  *  * 级别 命令(shell脚本绝对路径)
  分 时 日 月 周
  ```
## crontab时间示例

```
每分钟(10:01,10:02 ...)
* * * * *   或  */1 * * * *

每小时
0 * * * *

每天
0 0 * * *

每周
0 0 * * 0

每月
0 0 1 * *

每年
0 0 1 1 *

每天早上６点
0 6 * * *

每２小时
0 */2 * * *

每小时10分，40分
10,40 * * * *

每天下午4,5,6点的 1,2,3,4,5min
1,2,3,4,5 16,17,18 * * *
```
## 示例
每分钟向日志文件追加一行hello world

编写test.sh
```
echo "hello world " >> /var/test.logs
```
编写crontab 步骤
```shell
1.查看当前任务列表
crontab -l

2.进入crontab编辑界面
crontab -e

3.末尾加入
* * * * * sh test.sh的绝对路径
```
