## 内网穿透

在进行本地开发和调试时，如果需要连接到集群内的服务，但本地环境无法直接访问这些服务，此时可以利用 Ehco 实现流量的转发，从而方便地进行本地开发和调试。

### 示例场景

假设您需要从本地开发环境连接到位于内网的数据库服务，数据库的主机地址为 `xxx-rds.xxx.us-east-1.rds.amazonaws.com`。

#### 步骤 1: 在 Kubernetes 集群内部署 Ehco

首先，在 Kubernetes 集群中启动一个 Ehco pod，以便将流量从本地转发到指定的内网服务。启动 Ehco pod 的命令如下：

```bash
ehco -l 0.0.0.0:3306 -r xxx-rds.xxx.us-east-1.rds.amazonaws.com:3306
```

#### 步骤 2: 使用 kubectl port-forward 转发端口

接下来，使用 `kubectl port-forward` 命令将本地的 3306 端口转发到 Ehco pod 的 3306 端口。这样，本地应用就可以通过访问本地的 3306 端口来与内网服务进行通信。

```bash
kubectl port-forward pod/ehco-pod 3306:3306
```

#### 步骤 3: 本地客户端连接数据库

最后，您可以使用本地的数据库客户端工具连接到数据库。由于端口已经被转发，您可以像访问本地服务一样访问内网的数据库服务：

```bash
mysql -h 127.0.0.1 -P 3306 -u root -p
```

通过以上步骤，您就可以轻松地从本地环境连接到集群内的服务，进行开发和调试。

---

更多使用案例将持续更新中...
