---
title: "SCP 基础知识"
date: 2021-12-02
tags: ["Linux", "SSH", "SCP", "Notes"]
---

### 概述

SCP 是 SSH 提供的一个客户端程序，用来在两台主机之间加密传送文件（即复制文件）。
`scp` 是 secure copy 的缩写，相当于 `cp` 命令 + SSH。它的底层是 SSH 协议，默认端口是 22，相当于先使用 ssh 命令登录远程主机，然后再执行拷贝操作。
scp 主要用于以下三种复制操作。

1. 本地复制到远程。

2. 远程复制到本地。

3. 两个远程系统之间的复制。

使用 `scp` 传输数据时，文件和密码都是加密的，不会泄漏敏感信息。

### 基本语法

`scp` 的语法类似 `cp` 的语法。

```bash
scp source destination
```

上面命令中，_source_ 是文件当前的位置，_destination_ 是文件所要复制到的位置。它们都可以包含用户名和主机名。

- 本地文件复制到远程:

```bash
# 语法
scp SourceFile user@host:directory/TargetFile

# 示例
scp file.txt remote_username@10.10.0.2:/remote/directory

# 将本机的 documents 目录拷贝到远程主机，
# 会在远程主机创建 documents 目录
scp -r documents username@server_ip:/path_to_remote_directory

# 将本机整个目录拷贝到远程目录下
scp -r localmachine/path_to_the_directory username@server_ip:/path_to_remote_directory/

# 将本机目录下的所有内容拷贝到远程目录下
scp -r localmachine/path_to_the_directory/* username@server_ip:/path_to_remote_directory/
```

- 远程文件复制到本地:

```bash
# 语法
scp user@host:directory/SourceFile TargetFile

# 示例
scp remote_username@10.10.0.2:/remote/file.txt /local/directory

# 拷贝一个远程目录到本机目录下
scp -r username@server_ip:/path_to_remote_directory local-machine/path_to_the_directory/

# 拷贝远程目录下的所有内容，到本机目录下
scp -r username@server_ip:/path_to_remote_directory/* local-machine/path_to_the_directory/
scp -r user@host:directory/SourceFolder TargetFolder
```

- 两个远程系统之间的复制:

```bash
# 语法
scp user@host1:directory/SourceFile user@host2:directory/SourceFile

# 示例
scp user1@host1.com:/files/file.txt user2@host2.com:/files
```
