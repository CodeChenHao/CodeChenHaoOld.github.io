---
layout: post
title: Docker学习
description: Docker的相关概念、安装过程、操作命令等
tags: Docker
---

## 一、认识Docker

```
1.Docker是一种容器技术，用于将我们的应用以及应用运行的环境一起打包、发布，从而解决软件跨环境迁移的问题
2.容器是完全使用沙箱机制，容器之间相互隔离
3.容器性能开销极低
4.Docker分为社区版和企业版
5.Docker官网：https://www.docker.com/
6.Docker Hub地址： https://hub.docker.com/
```

## 二、Docker的安装

```shell
# 1、yum 包更新到最新 
yum update
# 2、安装需要的软件包， yum-util 提供yum-config-manager功能，另外两个是devicemapper驱动依赖的 
yum install -y yum-utils device-mapper-persistent-data lvm2
# 3、 设置yum源
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
# 4、 安装docker，出现输入的界面都按 y 
yum install -y docker-ce
# 5、 查看docker版本，验证是否验证成功
docker -v
```

## 三、配置Docker镜像加速器

```
由于Docker的远程仓库的服务器在国外，在下载镜像的时候速度比较慢，因此安装完Docker后先配置Docker的镜像加速器。镜像加速器的地址有很多，如中科大、阿里云、网易云、腾讯云。这里采用速度较快的阿里云。
步骤：
	1.登录阿里云
	2.点击"控制台"
	3.搜索"容器镜像服务"
	4.按"镜像加速器"s中的命令执行操作
```

```shell
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://vthkcplb.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

## 四、Docker的组成部分

```
Docker = deamon + image + container + registries
```

基本概念

```
image: 镜像，本质上就是一个文件系统，相当于"软件"
registries: 仓库，分为Docker Hub以及Private registry，用于存放镜像，相当于"应用商店"
container: 容器，由image产生，image相当于类，container相当于具体的实例
deamon: Docker的守护进程,表示Docker服务
```



## 五、操作Docker的守护进程——deamon

```
开启Docker服务：systemctl start docker
停止Docker服务：systemctl stop docker
重启Docker服务：systemctl restart docker
查看Docker服务的状态：systemctl status docker
设置Docker开机自启：systemctl enable docker
```

## 六、操作镜像——image

```
1.查看本地镜像列表：docker images
2.查看本地镜像的id：docker images -q
3.在docker hub仓库中搜索image: docker search 镜像名

4.从docker hub仓库中下载image: docker pull 镜像:版本 

5.删除本地镜像：docker rmi 镜像ID/镜像名:版本

注：
1.下载镜像时，未指定版本，则下载最新版本
2.删除所有本地镜像：docker rmi `docker images -q`
```

## 七、操作容器——contrainer

### 7.1	查看容器

```
查看运行状态中的容器：docker ps
查看所有容器：docker ps -a
查看所有容器的ID：docker ps -aq
```

### 7.2	创建并启动容器

```
docker run 参数

参数说明：
	-i：保持容器运行。通常与 -t 同时使用。加入it这两个参数后，容器创建后自动进入容器中，退出容器后，容器自动关闭。
	-t：为容器重新分配一个伪输入终端，通常与 -i 同时使用。
	-d：以守护（后台）模式运行容器。创建一个容器在后台运行，需要使用docker exec 进入容器。退出后，容器不会关闭。
	-it 创建的容器一般称为交互式容器，-id 创建的容器一般称为守护式容器
	--name：为创建的容器命名。
```

### 7.3	启动容器

```shell
docker start 容器ID/容器名字
```

### 7.4	停止容器

```shell
docker stop 容器ID/容器名字
```

### 7.5	删除容器

```shell
docker rm 容器ID/容器名字
```

### 7.6	查看容器的详细信息

```shell
docker inspect 容器ID/容器名字
```

## 八、容器的数据卷

### 8.1	数据卷的概念

```
思考：
	1.Docker 容器删除后，在容器中产生的数据还在吗？
	2.Docker 容器和外部机器可以直接交换文件吗？
	3.容器之间怎么进行数据交互？
	
解答：
	1.Docker 容器删除后，在容器中产生的数据也会随之销毁；
	2.Docker 容器和外部机器不可以直接交换文件；
	3.容器之间不能直接进行数据交互，需要通过宿主机；
```

```
为了解决上面的问题，因此引出了数据卷的概念。
数据卷是什么：
	1.数据卷是宿主机中的一个目录或文件
	2.当容器目录和数据卷目录绑定后，对方的修改会立即同步
	3.一个数据卷可以被多个容器同时挂载
	4.一个容器也可以被挂载多个数据卷
数据卷的作用：
	1.可以实现容器数据持久化
	2.可以实现外部机器和容器间接通信
	3.可以实现容器之间数据交换
```

### 8.2	配置数据卷

```
创建启动容器时，使用 –v 参数 设置数据卷
docker run ... –v 宿主机目录(文件):容器内目录(文件) ... 
```

```
注意事项：
        1. 目录必须是绝对路径
        2. 如果目录不存在，会自动创建
        3. 可以挂载多个数据卷
```

### 8.3	数据卷容器

```
数据卷容器，与普通的容器没有什么差别，只不过该容器配置了数据卷，然后其他的容器就可以通过该容器快速的配置相同的数据卷，这个时候这个容器就被称作数据卷容器。作用：帮助其他容器快速创建数据卷，实现多个容器挂载到同一数据卷
```

### 8.4	配置数据卷容器

```shell
1.创建启动c3数据卷容器，使用 –v 参数 设置数据卷
docker run –it --name=c3 –v /volume centos:7 /bin/bash 
```

```shell
2.创建启动 c1 c2 容器，使用 –-volumes-from 参数 设置数据卷
docker run –it --name=c1 --volumes-from c3 centos:7 /bin/bash

docker run –it --name=c2 --volumes-from c3 centos:7 /bin/bash  
```

