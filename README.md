# 基于FRP的内网穿透方案

## 相关资料

- [Docker实践：部署内网穿透frp - -零 - 博客园 (cnblogs.com)](https://www.cnblogs.com/-wenli/p/13951433.html)
- [文档 | frp (gofrp.org)](https://gofrp.org/docs/)

## 部署步骤

### 部署frp-server

```bash
$ mkdir -p /data/frps
$ cd /data/frps
$ vim frps.ini
[common]
bind_port = 7001
$ docker-compose -f frps.yml up -d 
$ docker-compose -f frps.yml logs -f 
Attaching to frps
frps    | 2021/05/30 07:43:04 [I] [root.go:200] frps uses config file: /etc/frp/frps.ini
frps    | 2021/05/30 07:43:04 [I] [service.go:192] frps tcp listen on 0.0.0.0:7001
frps    | 2021/05/30 07:43:04 [I] [root.go:209] frps started successfully
```

### 部署frp-client

```bash
$ mkdir -p /data/frpc
$ cd /data/frpc
$ vim frpc.ini
[common]
server_addr = x.x.x.x // 换成搭建frps的公网IP
server_port = 7001

[ssh]
type = tcp
local_ip = 127.0.0.1
local_port = 22
remote_port = 6000
$ docker-compose -f frpc.yml up -d 
$ docker-compose -f frpc.yml logs -f 

```

