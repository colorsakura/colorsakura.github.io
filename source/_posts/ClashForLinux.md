---
title: ClashForLinux
mathjax: true
date: 2021-09-13 20:32:19
tags: [Clahs, Linux, Tools]
---

Clash is a rule-based tunnel in Go.

### Features

- Local HTTP/HTTPS/SOCKS server with authentication support
  VMess, Shadowsocks, Trojan, Snell protocol support for remote connections.
- Built-in DNS server that aims to minimize DNS pollution attack impact, supports DoH/DoT upstream and fake IP.
- Rules based off domains, GEOIP, IP CIDR or ports to forward packets to different nodes.
- Remote groups allow users to implement powerful rules. Supports automatic fallback, load balancing or auto select node based off latency.
- Remote providers, allowing users to get node lists remotely instead of hardcoding in config.
- Netfilter TCP redirecting. Deploy Clash on your Internet gateway with iptables.
- Comprehensive HTTP RESTful API controller.

### Download

选择合适的版本下载即可。`https://github.com/Dreamacro/clash/releases`

### Install

```bash
wget https://github.com/Dreamacro/clash/releases/download/v1.7.0/clash-darwin-arm64-v1.7.0.gz
gzip - clash-darwin-arm64-v1.7.0.gz
```
