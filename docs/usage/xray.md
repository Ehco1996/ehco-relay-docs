# Xray 集成与用户流量统计

从 `v1.1.2` 版本开始，ehco 引入了对 [xray](https://github.com/XTLS/Xray-core) 后端的完整支持，允许用户通过标准的 xray 配置文件启动内置的 xray 服务。这项功能的配置关键字为 `xray_config`。

## 用户流量统计

此外，自 `v1.1.2` 起，ehco 支持通过 API 同步用户配置并上报用户流量数据，相关配置的关键字为 `sync_traffic_endpoint`。

### 配置同步

ehco 将每隔 60 秒发送一次 GET 请求至 `sync_traffic_endpoint`，以从该端点同步用户配置信息至 xray 服务端。期望的 API 返回格式如下所示：

```json
{
    "users": [
        {
            "user_id": 1,
            "method": "user1",
            "password": 1024,
            "level": 1024,
            "upload_traffic": 1024,
            "download_traffic": 1024,
            "protocol": "trojan/ss"
        },
        {
            "user_id": 2,
            "method": "user1",
            "password": 1024,
            "level": 1024,
            "upload_traffic": 1024,
            "download_traffic": 1024,
            "protocol": "trojan/ss"
        }
    ]
}
```

### 流量上报

同样，ehco 会每隔 60 秒向 `sync_traffic_endpoint` 发送一次 POST 请求，上报当前 xray 服务端所有用户的流量使用情况。请求的格式如下：

```json
{
    "data": [
        {
            "user_id": 1,
            "upload_traffic": 1024,
            "download_traffic": 1024
        },
        {
            "user_id": 2,
            "upload_traffic": 1024,
            "download_traffic": 1024
        }
    ]
}
```

### 配置文件限制

使用此功能时，对 xray 的完整配置文件有特定的限制条件：

-   必须在 xray 配置中启用 `stats` 和 `api` 功能。
-   Shadowsocks 入站配置（inbound）的 `tag` 必须设置为 `ss_proxy`。
-   Trojan 入站配置（inbound）的 `tag` 必须设置为 `trojan_proxy`。

### 配置示例

完整的配置示例可以参考以下链接：

-   Shadowsocks 单端口多用户配置：[xray_ss.json](https://github.com/Ehco1996/ehco/blob/master/examples/xray_ss.json)
-   Trojan 单端口多用户配置：[xray_trojan.json](https://github.com/Ehco1996/ehco/blob/master/examples/xray_trojan.json)

通过集成 xray 及其用户流量统计功能，ehco 为用户提供了一个更为强大且灵活的流量转发解决方案，从而满足更多样化的网络需求。
