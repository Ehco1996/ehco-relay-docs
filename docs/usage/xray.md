# Xray

从 `v1.1.2` 开始，ehco 内置了完整版本的 [xray](https://github.com/XTLS/Xray-core) 后端，可以通过标准的 xray 配置文件来启动内置的 xray server, 配置的 key 为 `xray_config`：

## 用户流量统计

从 `v1.1.2` 开始，ehco 支持通过 api 下方用户配置和上报用户流量，配置的 key 为 `sync_traffic_endpoint`：

ehco 会每隔 60s 发送一次 GET 请求，从 `sync_traffic_endpoint` 同步一次用户配置，到 xray server 里，期望的 API 返回格式如下：

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

ehco 会每隔 60s 发送一次 POST 请求至 `sync_traffic_endpoint` ，上报当前 xray server 所有用户的流量使用情况，发送的请求格式如下：

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

需要注意的是，如果想使用此功能，对 xray 的完整配置文件有如下限制

-   xray 配置文件必须开启 `stats` 和 `api` 功能
-   ss inbound 的 `tag` 必须为 `ss_proxy`
-   trojan inbound 的 `tag` 必须为 `trojan_proxy`

完整的例子可以参考:

-   单端口多用户的 ss [xray_ss.json](https://github.com/Ehco1996/ehco/blob/master/examples/xray_ss.json)
-   单端口多用户的 trojan [xray_trojan.json](https://github.com/Ehco1996/ehco/blob/master/examples/xray_trojan.json)
