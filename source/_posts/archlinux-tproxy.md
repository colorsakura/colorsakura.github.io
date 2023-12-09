---
title: archlinux-tproxy
date: 2023-12-09 23:29:18
tags: ["clash", "nftables", "archlinux"]
---

使用 Arch Linux 已经有一段时间了，浏览器使用 `Pac` 来控制代理确实简单方便，
但是有很多软件没办法方便地设置代理。所以需要花点时间配置一下透明代理。

主要使用的软件：`clash-meta`, `nftables`

---

## clash-meta 配置

```shell
# 安装
sudo pacman -S clash-meta

# 启动服务
sudo systemctl enable --now clash-meta

# 编辑配置
sudo vim /etc/clash-meta/config.yaml
```

启用透明代理 clash 所需要的配置

```yaml
routing-mark: 1
sniffer:
  enable: true
  sniffing:
    - tls
    - http
tproxy-port: 7893
dns:
  enable: true
  listen: "0.0.0.0:1053"
  enhanced-mode: redir-host
  default-nameserver:
    - 8.8.8.8
  nameserver:
    -
  fallback:
    -
  fallback-filter:
    geoip: true
    geoip-code: CN
    ipcidr:
      - 240.0.0.0/4
```

## 系统路由

```shell
ip rule add fwmark 1 table 100
ip route add local 0.0.0.0/0 dev lo table 100
```

## nftables 配置

### chnroute

```shell
curl 'http://ftp.apnic.net/apnic/stats/apnic/delegated-apnic-latest' | awk -F\| '/CN\|ipv4/ { printf("%s/%d\n", $4, 32-log($5)/log(2)) }'| sed ':label;N;s/\n/, /;b label'|sed 's/$/& }/g'|sed 's/^/define chnroute_ipv4 = { &/g' > ipv4-chnroute.nft
```

```shell
curl 'https://raw.githubusercontent.com/misakaio/chnroutes2/master/chnroutes.txt' | awk 'NR > 2 {printf"%s,\n", $1}' | sed ':label;N;s/\n/ /;b label'|sed 's/$/& }/g'|sed 's/^/define chnroute_ipv4 = { &/g' > ipv4-chnroute.nft
```

### nftables 规则

```nft
#!/usr/bin/nft -f

flush ruleset

# include "/etc/nftables.d/ipv4-whitelist.nft"
include "/etc/nftables.d/ipv4-private.nft"
include "/etc/nftables.d/ipv4-chnroute.nft"

define DIRECT-IPV4 = {
#     $whitelist_ipv4,
    $private_ipv4,
    $chnroute_ipv4,
}

table inet clash {
    chain clash-tproxy {
        # debug
        # meta l4proto { tcp, udp } meta nftrace set 1

        meta l4proto { tcp, udp } meta mark set 1 tproxy to :7893 accept
    }

    chain clash-mark {
        meta mark set 1
    }

    chain mangle-output {
        type route hook output priority mangle; policy accept;
        fib daddr type { unspec, local, anycast, multicast } accept
        ip daddr $DIRECT-IPV4 accept
        meta l4proto { tcp, udp } th dport 1-1024 mark != 1 ct direction original jump clash-mark
    }

    chain mangle-prerouting {
        type filter hook prerouting priority mangle; policy accept;
        meta l4proto { tcp, udp } th dport 53 accept
        fib daddr type { unspec, local, anycast, multicast } accept
        ip daddr $DIRECT-IPV4 accept
        # dport 1-1024 避免BT走代理
        # iif { lo } meta l4proto { tcp, udp } th dport 1-1024 ct direction original jump clash-tproxy
        iif { lo } meta l4proto { tcp, udp } ct direction original jump clash-tproxy
    }
}
```

```nft
# /etc/nftables.d/ipv4-private.nft
define private_ipv4 = {
	0.0.0.0/8,
	10.0.0.0/8,
	127.0.0.0/8,
	169.254.0.0/16,
	172.16.0.0/12,
	192.168.0.0/16,
	224.0.0.0/4,
	240.0.0.0/4
}
```
