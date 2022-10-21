---
title: Docker安装qbittorrent
author: BeiyanLuansheng
date: 2022-10-21 21:44:49 +0800
categories: [技术积累, Linux]
tags: [linux, docker, qbittorrent]
---


拉取 4.1.9 版本镜像

```shell
docker pull linuxserver/qbittorrent:4.1.9.99201911190849-6738-0b055d8ubuntu18.04.1-ls54
```

启动

```shell
docker run --name=byls-qb \
  --network host -d \
  -e WEBUI_PORT=9002 \
  -e TZ="Asia/Shanghai" \
  -v /home/byls/dockers/qbittorrent:/downloads \
  --restart always \
  linuxserver/qbittorrent:4.1.9.99201911190849-6738-0b055d8ubuntu18.04.1-ls54
```

- web端口使用9002
- 时区使用 上海
- 挂载下载目录 /downloads 到宿主机 /home/byls/dockers/qbittorrent

登陆

账号 admin
密码 adminadmin