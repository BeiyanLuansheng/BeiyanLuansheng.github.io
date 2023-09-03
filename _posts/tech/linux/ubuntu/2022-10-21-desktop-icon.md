---
title: Ubuntu 创建桌面快捷方式
author: BeiyanLuansheng
date: 2022-10-21 00:13:59 +0800
categories: [技术积累, Linux]
tags: [linux, ubuntu, system]
---


把解压的程序移动到 /opt 目录下
```
sudo mv jetbrains/ /opt
```

在 /usr/share/applications 目录下创建一个扩展名为 .desktop 的文件
```
cd /usr/share/applications
sudo touch idea.desktop
sudo vim idea.desktop
```

在文件中写入

```
[Desktop Entry]
Name=IDEA
Name[zh_CN]=IDEA
Comment=IDEA
Exec=/opt/jetbrains/idea-IU-222.3345.118/bin/idea.sh
Icon=/opt/jetbrains/idea-IU-222.3345.118/bin/idea.png
Terminal=false
Type=Application
Categories=Application;
Encoding=UTF-8
StartupNotify=true
```
