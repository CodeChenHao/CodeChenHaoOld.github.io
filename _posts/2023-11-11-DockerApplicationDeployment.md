---
layout: post
title: Docker应用部署
description: 通过Docker安装应用程序，包括Mysql、Tomacat、Nginx、Redis、Elasticsearch、Kibana
tags: Docker
---

### 一、部署MySQL

#### 1.1 通过Docker直接安装

##### 1.1.1 搜索镜像

```sh
docker search mysql
```

##### 1.1.2 拉取镜像

```sh
docker pull mysql:5.6
```

##### 1.1.3 部署应用

```sh
# 在/root目录下创建mysql目录用于存储mysql数据信息
mkdir ~/mysql
cd ~/mysql
```

```sh
docker run \
-d \
-p 3306:3306 \
--name=c_mysql \
-v /opt/docker-mysql/conf.d:/etc/mysql/conf.d \
-v /opt/docker-mysql/logs:/logs \
-v /opt/docker-mysql/data:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=root \
mysql:5.6
```

- 参数说明：
  - **-p 3306:3306**：将容器的 3306 端口映射到宿主机的 3306 端口。
  - **-v /opt/docker-mysql/conf.d:/etc/mysql/conf.d**：将宿主机的/opt/docker-mysql/conf.d 目录挂载到容器的 /etc/mysql/conf.d。配置目录
  - **-v /opt/docker-mysql/logs:/logs**：将宿主机的/opt/docker-mysql/logs目录挂载到容器的 /logs。日志目录
  - **-v /opt/docker-mysql/data:/var/lib/mysql** ：将宿主机的/opt/docker-mysql/data目录挂载到容器的 /var/lib/mysql 。数据目录
  - **-e MYSQL_ROOT_PASSWORD=123456：**初始化 root 用户的密码。

#### 1.2 通过Docker-Compose安装

##### 1.2.1 编写docker-compose.yml

```yaml
version: '3.1'
services:
  redis:
    restart: always
    image: mysql:5.6
    container_name: c_mysql
    ports:
      - 3306:3306
    volumes:
      - /opt/docker-mysql/conf.d:/etc/mysql/conf.d
      - /opt/docker-mysql/logs:/logs
      - /opt/docker-mysql/data:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: root
```

##### 1.2.2 通过docker-compose运行

```sh
docker-compose up -d
```



### 二、部署Tomcat

#### 2.1 通过Docker直接安装

##### 2.1.1 搜索镜像

```sh
docker search tomcat
```

##### 2.1.2 拉取镜像

```sh
docker pull tomcat
```

##### 2.1.3 部署应用

```sh
docker run \
-d \
--name=c_tomcat \
-p 8080:8080 \
-v /opt/docker-tomcat/webapps:/usr/local/tomcat/webapps \
tomcat 
```

- 参数说明：
  - **-p 8080:8080：**将容器的8080端口映射到主机的8080端口
  
    **-v /opt/docker-tomcat/webapps:/usr/local/tomcat/webapps：**将宿主机的/opt/docker-tomcat/webapps目录挂载到容器的/usr/local/tomcat/webapps


#### 2.2 通过Docker-Compose安装

##### 2.2.1 编写docker-compose.yml

```yaml
version: '3.1'
services:
  redis:
    restart: always
    image: tomcat
    container_name: c_tomcat
    ports:
      - 8080:8080
    volumes:
      - /opt/docker-tomcat/webapps:/usr/local/tomcat/webapps
```

##### 2.2.2 通过docker-compose运行

```sh
docker-compose up -d
```




### 三、部署Nginx

#### 3.1 通过Docker直接安装

##### 3.1.1 搜索镜像

```sh
docker search nginx
```

##### 3.1.2 拉取镜像

```sh
docker pull nginx
```

##### 3.1.3 部署

```sh
docker run \
-d \
--name=c_nginx \
-p 80:80 \
-v /opt/docker-nginx/conf.d:/etc/nginx/conf.d \
-v /opt/docker-nginx/logs:/var/log/nginx \
-v /opt/docker-nginx/html:/usr/share/nginx/html \
nginx
```

3.1.4 配置应用


```shell
cd /opt/docker-nginx/conf.d
vim nginx.conf
```
```shell
user  nginx;
worker_processes  1;

error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    #gzip  on;

    include /etc/nginx/conf.d/*.conf;
}


```

#### 3.2 通过Docker-Compose安装

##### 3.2.1 编写docker-compose.yml

```yaml
version: '3.1'
services:
  nginx:
    restart: always
    image: c_nginx
    container_name: nginx
    ports:
      - 80:80
    volumes:
      - /opt/docker-nginx/conf.d:/etc/nginx/conf.d
      - /opt/docker-nginx/logs:/var/log/nginx
      - /opt/docker-nginx/html:/usr/share/nginx/html
```

##### 3.2.2 通过docker-compose运行

```sh
docker-compose up -d
```



### 四、部署Redis

#### 4.1 通过Docker直接安装

##### 4.1.1 搜索镜像

```shell
docker search redis
```

##### 4.1.2 拉取镜像

```shell
docker pull redis:5.0
```

##### 4.1.3 部署应用

```shell
docker run \
-d \
--name=c_redis \
-p 6379:6379 \
redis:5.0
```

#### 4.2 通过Docker-Compose安装

##### 4.2.1 编写docker-compose.yml

```yaml
version: '3.1'
services:
  redis:
    restart: always
    image: redis:5.0
    container_name: c_redis
    ports:
      - 6379:6379
```

##### 4.2.2 通过docker-compose运行

```sh
docker-compose up -d
```

### 五、部署Elastisearch与Kibana

#### 5.1 通过Docker-Compose安装

```sh
# 为了解决启动elasticsearch报错日志信息
# max virtual memory areas vm.max_map_count 【65530】 is too low, increase to at least 【262144】
# 修改宿主机的配置文件/etc/sysctl.conf
vim /etc/sysctl.conf
# 添加下面类型
vm.max_map_count=655360
# 执行下面的命令
sysctl -p
```

##### 5.1.1 编写docker-compose.yml

```yaml
version: '3.1'
services:
  elasticsearch:
    restart: always
    image: elasticsearch:6.5.4
    container_name: elasticsearch
    ports:
      - 9200:9200
  kibana:
    restart: always
    image: kibana:6.5.4
    container_name: kibana
    ports:
      - 5601:5601
    environment:
      - elasticsearch_url=http://192.168.3.123:9200
    depends_on:
      - elasticsearch
```

##### 5.1.2 通过docker-compose运行

```sh
docker-compose up -d
```

##### 5.1.3 安装IK分词器

```text
# IK分词器下载地址
https://github.com/infinilabs/analysis-ik/releases/download/v6.5.4/elasticsearch-analysis-ik-6.5.4.zip
```

```
# 将下载后zip上传到宿主机,然后通过unzip 命令解压,解压后再通过docker cp命令将解压后的文件夹复制到elasticsearch容器的/usr/share/elasticsearch/plugins这个目录下,然后通过docker restart 命令重启elasticsearch容器
```

解压

![image-20240916193128462](/images/posts/2023-11-11-DockerApplicationDeployment/image-20240916193128462.png)

将解压后的文件夹复制到elasticsearch容器

![image-20240916193216429](/images/posts/2023-11-11-DockerApplicationDeployment/image-20240916193216429.png)

重启elasticsearch容器

![image-20240916193229497](/images/posts/2023-11-11-DockerApplicationDeployment/image-20240916193229497.png)

##### 5.1.4 测试

![image-20240916193408575](/images/posts/2023-11-11-DockerApplicationDeployment/image-20240916193408575.png)

### 六、部署RabbitMQ

#### 6.1 通过Docker-Compose安装

##### 6.1.1 编写docker-compose.yml

```yaml
version: '3.1'
services:
  rabbitmq:
    restart: always
    image: rabbitmq:management
    container_name: rabbitmq
    ports:
      - 5672:5672
      - 15672:15672
    volumes:
      - /opt/docker-rabbitmq/data:/var/lib/rabbitmq
```

##### 6.1.2 通过docker-compose运行

```sh
docker-compose up -d
```

