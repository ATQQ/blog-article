# Docker

* docker images:查看本地镜像
* docker search sourceName :搜索镜像
  * docker search --filter "is-official=true" sourceName:过滤官方镜像
  * docker search --filter "is-official=true" node:官方node镜像
  * docker search --filter stars=10 sourceName:星星大于10的指定镜像资源
* docker pull sourceName :下载指定资源镜像
  * docker pull sourceName:版本号
  * docker pull centos:7    :下载centos7镜像
* docker tag centos:7 mycentos:first  :修改镜像名称(小写)
* docker rmi sourceName :删除本地镜像
  * docker rmi centos:7 :删除centos7镜像 
* docker run -it sourceName|imageId:tag :启动容器(不加:tag 默认运行本地最新的:latest 没有就远程下载)
  * -i :以交互模式运行容器
  * -t :为容器重新分配一个终端
  * -d :后台运行
* docker ps :查看运行的容器
  * -a :显示所有(包括已经关闭的)
* docker stop ContainerId:停止后台运行的容器
* docker run -itd --name=testcentos centos:7 :指定运行容器的name,方便停止
  * docker stop testcentos: 用name停止
  * docker start testcentos: 启动
  * docker restart testcentos: 重启
  * dokcer rm testcentos :删除容器(必须停止的容器)
    * -f :强制删除
  * docker inspect testcentos: 查看容器的详细信息
* docker exec -it mycentos /bin/bash:进入后台运行的centos容器
* docker stop $(docker ps -q):停止所有的运行中的容器
  * docker ps -q:查询所有运行中容器的id
  * docker ps -aq:所有容器的id
* docker start ${docker ps -a -q}:运行所有容器
* docker cp /root/test.txt imageName:/home/ :拷贝本地文件/root/test.txt到容器中的/home/目录下
* docker cp imageName:/home/test.txt /root :拷贝容器中的文件到宿主机上
* docker run -itd -v 要挂载目录:容器中的目录 imageName:tag:挂载指定目录到指定容器中
### 配置阿里云加速器加速镜像下载(可以去阿里云控制台寻找专属的)
```text
mkdir -p /etc/docker
tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://v2ltjwbg.mirror.aliyuncs.com"]
}
EOF
systemctl daemon-reload
systemctl restart docker
```