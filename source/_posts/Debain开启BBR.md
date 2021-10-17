---
title: "Debian 开启BBR"
date: 2021-10-17
tags: ["Linux", "Debian", "Debian11", "Notes", "BBR"]
---

由于 Debian10 默认的内核就是 4.19 版本的内核而且编译了 TCP BBR 模块，所以可以直接通过参数开启。

新的 TCP 拥塞控制算法 BBR (Bottleneck Bandwidth and RTT) 可以让服务器的带宽尽量跑满，并且尽量不要有排队的情况，让网络服务更佳稳定和高效。

```shell
# 修改系统变量
echo net.core.default_qdisc=fq >> /etc/sysctl.conf
echo net.ipv4.tcp_congestion_control=bbr >> /etc/sysctl.conf

# 保存生效
sysctl -p

# 执行
sysctl net.ipv4.tcp_available_congestion_control
# 显示结果
# net.ipv4.tcp_available_congestion_control = reno cubic bbr

# 检测
lsmod | grep bbr
```
