---
title: OES之十：Dcoker部署
author: BeiyanLuansheng
date: 2022-01-11 14:09:34 +0800
categories: [技术积累, Spring-OES]
tags: [Dokcer, Redis, MySQL]
---

## 部署 MinIO

拉取镜像

```shell
sudo docker pull minio/minio
```

启动

```shell
sudo docker run -p 8081:9000 -p 8082:9001 --name oesminio -v /home/admin/minio/data:/data -e MINIO_ROOT_USER=oesminio -e MINIO_ROOT_PASSWORD=oesminioadmin -d minio/minio server /data --console-address ":9001"
```

- 启动容器名为 oesminio 的 minio/minio 镜像

- 挂在数据文件到 `/home/admin/minio/data` 目录下

- 启动控制台端口为 9001，映射到 8082

- 访问端口为 9000，映射到 8081

## 部署 Redis

拉取镜像

```shell
sudo docker pull redis
```

启动

```shell
sudo docker run -p 6379:6379 --name oesredis -v /home/admin/redis/data:/data -v /home/admin/redis/redis.conf:/etc/redis/redis.conf -d redis redis-server /etc/redis/redis.conf --appendonly yes
```

- 启动容器名为 oesredis 的 redis 镜像

- 挂载数据文件到 `/home/admin/redis/data` 目录下

- 挂在配置文件到 `/home/admin/redis/redis.conf`，并使用配置文件启动

- 服务端口为 3306，映射到 3306

- `--appendonly yes` 开启持久化存储

## 部署 MySQL

拉取镜像

```shell
sudo docker pull mysql
```

启动

```shell
sudo docker run -p 3306:3306 --name oesmysql -v /home/admin/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=oesmysqladmin -d mysql
```

- 启动容器名为 oesredis 的 redis 镜像

- 挂载数据文件到 `/home/admin/mysql/data` 目录下

- 挂在配置文件到 `/home/admin/redis/redis.conf`，并使用配置文件启动

- 服务端口为 3306，映射到 3306

> 远程客户端连接时需要设置连接参数 `allowPublicKeyRetrieval=true`
>
> 否则报错 `Public Key Retrieval is not allowed`

## 部署 OES

### Docker 打包


### 镜像上传


### 镜像拉取


### 启动