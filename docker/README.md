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
* docker commit -a "author" -m "description" containerId myImageName:Tag :基于现有的镜像创建自己的镜像
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


## Dockerfile
使用Dockerfile构建镜像

dockerfile
```dockerfile
# this is a dockerfile
FROM centos:7 # 基于centos:7镜像构建
MAINTAINER sugar engineerzjl@foxmail.com #作者信息
RUN echo "start build image" # 运行命令
WORKDIR /home/sugar #工作目录
COPY test.txt /home/sugar # 复制文件
RUN yum install -y net-tools # 运行安装命令
```
开始构建末尾的一点代表当目录
```shell
docker build -t mycentos:2 .
```
### 常用指令
```dockerfile
FROM    #基于哪个镜像
MAINTAINER 作者信息
COPY    #复制文件(单纯的复制,不做其他操作)
ADD     #复制文件如果是 .tar.gz 还会解压
WORKDIR #工作目录
ENV     #设置环境变量
EXPOSE  #暴露容器端口
RUN     #执行命令  作用于镜像层面

# 有多条时 在容器启动时执行最后一条
ENTRYPOINT  #执行后面的命令 作用于容器层面
CMD     #指定后面的命令 作用于容器层面 (会被传参覆盖)
  docker run imageName:tag params
```
命令格式
* shell:RUn echo "hello world
* exec:RUN ["yum","install","-y","net-tools"]

## 镜像分层结构
查看镜像的构建历史
```shell
docker history imageName:tag
```
镜像只读,容器可读可写

## 构建java环境
```
解压
tar -xf jdkxxxx
移动 jdk /usr/local
mv jdk /usr/local/jdk
配置环境变量
vi /etc/profile
加入已下内容在末尾
export JAVA_HOME=/usr/local/jdk
export JRE_HOME=$JAVA_HOME/jre
export CLASSPATH=$JAVA_HOME/lib:$JRE_HOME/lib:$CLASSPATH
export PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH

生效环境变量
source /etc/profile
检验
java -version

拷贝tomcat到指定目录中然后在bin目录中找到startup.sh启动
需要关闭防火墙才能访问(也可放行端口)
```
dockerfile构建
```dockerfile
FROM centos:7
ADD jdk-8u211-linux-x64.tar.gz /usr/local
RUN mv /usr/local/jdk1.8.0_211 /usr/local/jdk8
ENV JAVA_HOME=/usr/local/jdk8
ENV JRE_HOME=$JAVA_HOME/jre
ENV CLASSPATH=$JAVA_HOME/lib:$JRE_HOME/lib:$CLASSPATH
ENV PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH
ADD apache-tomcat-8.5.35.tar.gz /usr/local
RUN mv /usr/local/apache-tomcat-8.5.35 /usr/local/tomcat
EXPOSE 8080
ENTRYPOINT ["/usr/local/tomcat/bin/catalina.sh","run"]
```
启动
```shell
docker run -itd -p 8081:8080 -v /root/tomcatWebapps:/usr/local/tomcat/webapps/ROOT centos:java /bin/bash
```

# 构建nginx镜像
dicjerfile
```dockerfile
FROM centos:7
ADD nginx-1.16.0.tar.gz /usr/local
COPY nginx_install.sh /usr/local
RUN sh /usr/local/nginx_install.sh
EXPOSE 80
```
shell
```sh
#!/bin/bash
yum install -y gcc gcc-c++ make pcre pcre-devel zlib zlib-devel
cd /usr/local/nginx-1.16.0
./configure --prefix=/usr/local/nginx && make && make install
```
运行
```sh
docker run -itd -p 8081:80 mycentos:nginx /usr/local/nginx/sbin/nginx -g "daemon off;"
```
经测试可以后台启动容器后 进入容器再打开nginx
```
docker exec -it containerId /bin/bash
cd /usr/local/nginx/sbin
./nginx
```

## mysql镜像
拉取
```sh
docker pull mysql:5.7
```
后台运行
```sh
docker run --name test-mysql -p 3307:3306 -e MYSQL_ROOT_PASSWORD=a123456 -d mysql:5.7
```
进入容器(并设置字符编码)
```sh
docker exec -it test-mysql env LANG=C.UTF-8 /bin/bash
```

## 网络
* bridge  桥接
* host    主机
* none    
