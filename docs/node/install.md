---
title: 部署节点
---

# 部署节点

**可以通过以下几种方式之一获取最新版本的 ehco：**

-   [GitHub Releases](https://github.com/Ehco1996/ehco/releases)
-   [Linux 服务端部署脚本](#linux)
-   [Docker 镜像](#docker)

## Linux 部署脚本

我们提供了一个 bash 脚本，可以在常见的 Linux 发行版上自动下载最新版本的 ehco 并配置 systemd 服务。

安装或升级到最新版本：

```bash
bash <(curl -fsSL https://get.ehco-relay.cc) -i --config <config endpoint>
```

移除 ehco:

```bash
bash <(curl -fsSL https://get.ehco-relay.cc) --remove
```

请注意: 由于脚本会将 ehco 安装到 `/usr/local/bin/ehco` 所以请以 root 用户运行脚本。
对于非 root 用户，可以使用 `sudo` 命令。

```bash
# 下载脚本并赋予执行权限
curl -fsSL -o get-ehco.sh https://get.ehco-relay.cc
chmod +x get-ehco.sh

# 安装
sudo ./get-ehco.sh -i --config "config endpoint"

# 卸载
sudo ./get-ehco.sh --remove
```

## Docker 镜像

Docker Hub: <https://hub.docker.com/ehco1996/ehco>

### Compose 示例

```yaml
version: "3.9"
services:
    hysteria:
        image: ehco1996/ehco
        container_name: ehco
        restart: always
        network_mode: "host"
        command: ["ehco", "-c", "config.json"]
```

## 关于配置文件

[Ehco-Relay](https://ehco-relay.cc) 提供了配置文件的 API，在网站上配置好托管节点后，可以通过 API 获取配置文件。

如何获取请参考 [配置页面](manage.md)
