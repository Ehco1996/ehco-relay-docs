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

脚本支持如下功能

```bash
bash <(curl -fsSL https://get.ehco-relay.cc) -h

Usage: get-ehco.sh [options]

Options:
  -h, --help          Show this help message and exit.
  -v, --version       Specify the version to install.
  -i, --install       Install the Ehco.
  -c, --config         Specify the configuration file path or api endpoint.
  -r, --remove        Remove the Ehco.
  -u, --check-update  Check And Update if an update is available.
  --cf-proxy          Use cloudflare proxy to download/update ehco bin
```

### 安装 ehco

要安装或升级到最新版本的 ehco，请运行以下命令：

```bash
bash <(curl -fsSL https://get.ehco-relay.cc) -i --config <API 对接地址>
```

> **API 对接地址** 需要先在 [Ehco-Relay](https://ehco-relay.cc) 上配置好您的节点，然后在节点管理页面复制 。具体请参考 [配置页面](manage.md)。

**注意：** `--config <API 对接地址>` 参数是必须的，并且需要包含在引号中，一个例子如下：

```bash
bash <(curl -fsSL https://get.ehco-relay.cc) -i --config "https://ehco-relay.cc/api/v1/config/1/"
```

### 更新 ehco

要检查并更新到最新版本的 ehco，请运行以下命令：

```bash
bash <(curl -fsSL https://get.ehco-relay.cc) -u
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
sudo ./get-ehco.sh -i --config "API 对接地址"

# 使用 sudo 来卸载
sudo ./get-ehco.sh --remove
```

### 查看日志

```bash
journalctl -u ehco -f
```

## 从源码构建

如果您想从源码构建 ehco，可以按照以下步骤操作：

```bash
git clone git@github.com:Ehco1996/ehco.git

cd ehco

make build

cp dist/ehco /usr/local/bin

ehco -h
```

ehco 内置了 install 命令，方便快速安装 systemd 服务。

```bash
ehco install

# 会以系统默认编辑器弹出对应的配置文件并安装到 `/etc/systemd/system/ehco.service`
```

## 通过 docker 部署

您也可以通过 Docker 来部署 ehco，Docker Hub 上的镜像地址为：<https://hub.docker.com/r/ehco1996/ehco> 。

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
