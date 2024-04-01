---
title: 部署节点
---

# 部署节点

在本节中，我们将介绍如何获取并部署最新版本的 ehco，以确保您能够高效地使用我们的流量转发服务。您可以通过以下几种方式之一来获取 ehco：

-   通过 [GitHub Releases](https://github.com/Ehco1996/ehco/releases) 直接下载。
-   使用我们为 Linux 服务端准备的自动部署脚本。
-   通过 [Docker 镜像](#docker) 来部署。

## Linux 部署脚本

为了简化在 Linux 上的部署过程，我们提供了一个 bash 脚本。这个脚本能够自动下载 ehco 的最新版本，并为其配置 systemd 服务，以便您可以轻松管理 ehco 服务。

### 安装或升级 ehco

要安装或升级到最新版本的 ehco，请运行以下命令：

```bash
bash <(curl -fsSL https://get.ehco-relay.cc) -i --config <配置文件端点>
```

### 移除 ehco

如果您需要移除 ehco，可以执行以下命令：

```bash
bash <(curl -fsSL https://get.ehco-relay.cc) --remove
```

**注意：**该脚本默认将 ehco 安装到 `/usr/local/bin/ehco`

因此请使用 root 用户执行该脚本。如果您是非 root 用户，可以通过 `sudo` 命令来执行。

示例：

```bash
# 下载脚本并赋予执行权限
curl -fsSL -o get-ehco.sh https://get.ehco-relay.cc
chmod +x get-ehco.sh

# 使用 sudo 来安装
sudo ./get-ehco.sh -i --config "配置文件端点"

# 使用 sudo 来卸载
sudo ./get-ehco.sh --remove
```

## Docker 镜像

您也可以通过 Docker 来部署 ehco，Docker Hub 上的镜像地址为：<https://hub.docker.com/ehco1996/ehco> 。

### Docker Compose 示例

以下是一个使用 Docker Compose 部署 ehco 的示例配置：

```yaml
version: "3.9"
services:
    ehco:
        image: ehco1996/ehco
        container_name: ehco
        restart: always
        network_mode: "host"
        command: ["-c", "config.json"]
```

## 配置文件

ehco 的配置可以通过 [Ehco-Relay](https://ehco-relay.cc) 提供的 API 来获取。您只需在网站上配置您的托管节点，然后即可通过 API 获取配置文件。详细信息请参考 [配置页面](manage.md)。
