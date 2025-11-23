---
layout: post
title: Docker2Compose
description: 一键将Docker命令转化为Compose的神器
tags: Docker
---

## 一、为什么需要Docker2Compose

```
在Docker的日常使用中,我们常常遇到这样的场景:
通过反复调试docker run命令启动了一个包含复杂参数的容器（比如挂载了多个卷、设置了环境变量、配置了网络端口）,最终需要将其转化为可复用的docker-compose.yml文件。
手动转换不仅耗时，还容易遗漏参数。Docker2Compose正是为解决这一痛点而生——它能将复杂的docker run命令智能转换为完整的Docker Compose配置！
```

## 二、安装Docker2Compose

```shell
cd /opt
mkdir docker-docker2compose
cd docker-docker2compose
vim docker-compose.yml
```

docker-compose.yml文件内容

```yml
services:
  d2c:
    image: crpi-xg6dfmt5h2etc7hg.cn-hangzhou.personal.cr.aliyuncs.com/cherry4nas/d2c:latest
    container_name: d2c
    ports:
      - "5000:5000"
    environment:
      - TZ=Asia/Shanghai
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock:ro
      - /opt/docker-docker2compose/d2c/compose:/app/compose
      - /opt/docker-docker2compose/d2c/logs:/app/logs
      - /opt/docker-docker2compose/d2c/config:/app/config
```

```shell
docker compose up
```

## 三、访问并使用Docker2Compose

```
http://ip:5000
```

![image-20251123220000094](/images/posts/2025-11-23-Docker2Compose/image-20251123220000094.png)
