---
title: "qBittorrent 自动更新 trackers"
date: 2021-10-17
tags: ["Linux", "Debian", "Debian11", "Notes", "NAS", "qBittorrent", "BT", "Shell"]
---

### 脚本内容

废弃

```python
#!/usr/bin/env python3
import requests as r

# 配置文件绝对路径
conf = "/home/chauncey/.config/qBittorrent/qBittorrent.conf"

url = "https://cdn.jsdelivr.net/gh/ngosang/trackerslist@master/trackers_best.txt"

trackers = r.get(url).text

with open(conf, "rw") as f:
    c = f.read()
    p = f.read().find("Bittorrent\TrackersList=")
    f.write(c[:p] + trackers +c[p:])
```
