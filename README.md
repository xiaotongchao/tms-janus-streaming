# tms-janus-streaming

基于 janus-gateway 实现的流媒体服务器。

## coturn

为了解决点对点通信，需要支持穿越 Nat，配置自己的 turn 服务器（coturn）。

参考：https://github.com/coturn/coturn

## janus

Janus-gateway 是开源的 WebRTC 服务器。

参考：https://janus.conf.meetecho.com

## ue_client

在 nginx 中运行前端代码。

# 环境准备

项目目录下新建`docker-compose.override.yml`文件。

```
version: "3.7"
services:
  janus:
    network_mode: "bridge"
    ports:
      - "8088:8088"
    # volumes:
    #   - /etc/letsencrypt:/etc/letsencrypt
    env_file:
      - ./local.env

  ue_client:
    # volumes:
    #   - /etc/letsencrypt:/etc/letsencrypt
    env_file:
      - ./local.env
```

服务 janus 默认使用的网络模式未`host`，但是在 Mac 和 Windows 环境下不支持，所以需要修改网络模式，并指定需要映射的端口。

如果服务器安装了 ssl 证书，janue 和 nginx 需要开启 https 端口，需要将存放证书的目录挂载到容器中。

如果需要开启 stun 和 ssl，需要给环境变量赋值。新建文件`local.env`（可以根据需要命名），指定使用这个文件。

```
# ssl证书位置
ssl_certificate=
ssl_certificate_key=

# janus stun-server
stun_server=stun.stunprotocol.org:3478
```

# 运行

> docker-compose up --build

在浏览器中打开：https://yourdomain:8080
