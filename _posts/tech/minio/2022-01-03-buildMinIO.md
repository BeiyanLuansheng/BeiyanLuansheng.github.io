---
title: MiniIO 入门教程
author: BeiyanLuansheng
date: 2022-01-03 13:54:02 +0800
categories: [技术积累, MinIO]
tags: [docker, MinIO, file]
---

## Minio 简介

MinIO 官网 <https://min.io/>

MinIO 官方手册 <https://docs.min.io/docs/>

MinIO 官方中文手册 <http://docs.minio.org.cn/docs/> 中文文档有滞后性

MinIO 开源地址 <https://github.com/minio/minio>

MiniIO 是一个开源的对象存储服务，适合于存储大容量非结构化的数据，例如图片、视频、日志文件、备份数据等，本文将介绍如何使用MinIO 搭建一个对象存储服务。

## 搭建服务器

首先服务器上需要安装 Dokcer（便于操作）

拉取镜像

```shell
docker pull minio/minio
```

启动镜像，把文件存储目录挂载到宿主机上 


```shell
docker run -p 9090:9000 -p 9001:9001 --name minio -v /mydata/minio/data:/data -e MINIO_ROOT_USER=minioadmin -e MINIO_ROOT_PASSWORD=minioadmin -d minio/minio server /data --console-address ":9001"
```

### 控制台

启动之后我们就可以通过访问 http://host:9001 进入控制台，9001端口是在启动命令中加入的参数 `--console-address ":9001"` 否则会随机端口启动，并且我们把 docker 的9001端口映射到了物理机的 9001 端口，所以可以直接访问物理机的 9001 端口进入控制台

![image-20220110113311745](/minio/image-20220110113311745.png)

启动时设置了账号密码参数 `-e MINIO_ROOT_USER=minioadmin -e MINIO_ROOT_PASSWORD=minioadmin` 所以可以用此账号登录控制台

![image-20220110113433209](/minio/image-20220110113433209.png)


## 目录管理

上一节的命令中，我们把 `/data` 目录挂载到了物理机的 `/mydata/minio/data` 目录下，这里就是MinIO存储的根目录

在MinIO中，有两个概念，一个是 桶(Bucket)，其实际上就是在存储根目录下建立不同的文件夹，建立一个名为 avatar 的桶实际上就是创建路径 `/mydata/minio/data/avatar`

另外一个就是文件存储名称，名称中与一般路径相同，可以添加以 `/` 分割的路径组织文件存储

## 访问

可以直接在线访问 .jpg, .png 等图片文件；可以直接在线访问 .mp4 等视频文件

首先要保证桶的访问策略是公开的

![image-20220110112619917](/minio/image-20220110112619917.png)

随便往这个桶上传一个文件，然后就可以直接使用保存路径访问这个桶里的文件

MinIO 的默认访问端口是 9000，而我们在启动Docker 时把 9000 映射到了 9090，所以我们访问的是 9090

![image-20220110113816138](/minio/image-20220110113816138.png)

如果文件类型不支持在线预览则会直接下载

## 参考

<https://mp.weixin.qq.com/s/qHjOEeQ3CaA0U4a2YBi3Pw>

<https://tonybai.com/2020/03/16/build-high-performance-object-storage-with-minio-part1-prototype/>

<https://juejin.cn/post/6997202001834541069>