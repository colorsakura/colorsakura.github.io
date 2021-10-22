---
title: "Debian 基础篇(一)"
date: 2021-10-02
tags: ["Linux", "Debian", "Debian11", "Notes"]
---

### 时间&&时区&&语言

timedatectl是一个命令行工具，它允许你查看并且修改系统时间和日期。它在所有现代的基于 systemd 的 Linux 系统中都可以使用。

- 查看当前时区 `timedatectl`

- 查看所有时区 `timedatectl list-timezones`

- 设置时区 `timedatectl set-timezone your_time_zone`  中国“Asia/Shanghai”

### 解压缩

