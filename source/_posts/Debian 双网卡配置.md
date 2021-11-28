---
title: "Debian 双网卡配置"
date: 2021-11-28
tags: ["Linux", "Debian", "Debian11", "Notes", "Network", "NetworkManager"]
---

### 概述

最近组装了一台自用 NAS，双网卡。一个 2.5G 有线，一个千兆 WIFI。系统使用 Debian11，有线连接笔记本，搭建局域网，WIFI 连接路由器的，使用外网。

### 问题

使用 Debian11 时，由于网络布局问题，NAS 无法直接路由器。本地安装 Debian11 时，确实网络教程中使用的配置工具。且感觉 Linux 的网络管理工具很多，有些会冲突只能使用其中之一。由于后期使用 cockpit 管理 NAS，所以最好选择使用 NetworkManager 管理网络。

### 操作记录

1. 安装 Debian11，后续写安装教程。

2. 官网下载相对于的 WIFI 网卡驱动包。使用 U 盘安装。

```bash
# 挂载U盘
mkdir /mnt/usb # 创建挂载位置
fdisk -l # 查看U盘路径
mount /dev/sdb /mnt/usb # 不要弄错了
dpkg -i package-name # 记得进入相应路径
```

> 请全程使用 root 用户安装。

3. 配置WIFI。


