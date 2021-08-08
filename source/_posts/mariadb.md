---
title: "MariaDB安装配置"
date: 2021-08-08
tags: ['MariaDB', 'Navicat', '数据库']
---

### MariaDB 背景

　　MariaDB 是 MySQL 关系数据库管理系统的一个复刻，由社区开发，有商业支持，旨在继续保持在 GNU GPL 下开源。MariaDB 的开发是由 MySQL 的一些原始开发者领导的，他们担心甲骨文公司收购 MySQL 后会有一些隐患。

　　MariaDB 打算保持与 MySQL 的高度兼容性，确保具有库二进制奇偶校验的直接替换功能，以及与 MySQL API 和命令的精确匹配。MariaDB 自带了一个新的存储引擎 Aria，它可以替代 MyISAM，成为默认的事务和非事务引擎。它最初使用 XtraDB 作为默认存储引擎，并从10.2版本切换回 InnoDB。

　　它的首席开发人员是米卡埃尔·维德纽斯，他是 MySQL AB 的创始人之一，也是 Monty Program AB 的创始人。2008年1月16日，MySQL AB 宣布它已经同意被 Sun 微系统集团以大约10亿美元的价格收购。该项收购已于2008年2月26日完成。MariaDB 是以 Monty 的小女儿 Maria 命名的，就像 MySQL 是以他另一个女儿 My 命名的一样。

### MariaDB 安装配置

- Linux：
默认使用发行版的版本就行了。

```bash
sudo apt-get install mariadb-server
```

- 初始化 MariaDB：

```bash
sudo mysql_secure_installation
```

然后根据提示完成相应的设置即可。

- 开启远程访问：

```bash
sudo mysql -u root -p
# 输入密码进入数据库
grant all privileges on *.* to 'root'@'%' identified by 'password'; # % 为允许所有ip访问 password 替换成设置的密码
flush privileges;
```

修改 MariaDB 配置文件

```bash
sudo vim /etc/mysql/mariadb.conf.d/50-server.cnf
```

注释掉`bind-address=127.0.0.1`
重启服务

```bash
sudo systemctl restart mariadb.service
```

### 数据库图形软件

强烈推荐使用 [Navicat](https://www.navicat.com.cn/)。
