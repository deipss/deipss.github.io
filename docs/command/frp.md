---
layout: default
title: frp
parent: Command

---

# service 
```shell
[common]
bind_port = 7000
token = ---
dashboard_port = 7500
dashboard_user = ---
dashboard_pwd = ---
```


# client
```shell
[common]
server_addr = x7.xx0.48.xxx
server_port = 7000
token = ---
admin_addr = 127.0.0.1
admin_port = 7400
[ssh]
type = tcp
local_ip = 127.0.0.1
local_port = 22
remote_port = 6000
[ollama]
type = tcp 
local_ip = 127.0.0.1
local_port = 11434
remote_port = 11434
```
- 启动脚本  nohup ./frpc -c frpc.ini > nohup.log 2>&1 &
- 热更新 curl http://127.0.0.1:7400/api/reload
- 配置文件 
  - https://github.com/fatedier/frp/blob/dev/conf/frpc_full_example.toml
  - https://github.com/fatedier/frp/blob/773169e0c44070facd652c4b8b3fe6e6ff4d78f3/conf/legacy/frps_legacy_full.ini