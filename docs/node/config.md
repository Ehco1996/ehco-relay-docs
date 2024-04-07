## 配置文件格式

一个完整的配置文件如下

```json
{
    "web_port": 9000,
    "web_token": "",
    "enable_ping": false,
    "log_level": "debug",
    "reload_interval": 5,
    "relay_configs": [
        {
            "listen": "127.0.0.1:1234",
            "listen_type": "raw",
            "transport_type": "raw",
            "label": "relay1",
            "tcp_remotes": ["0.0.0.0:5201"],
            "udp_remotes": ["0.0.0.0:5201"]
        },
        {
            "listen": "127.0.0.1:1235",
            "listen_type": "raw",
            "transport_type": "ws",
            "tcp_remotes": ["ws://0.0.0.0:2443"],
            "udp_remotes": ["0.0.0.0:5201"]
        },
        {
            "listen": "127.0.0.1:1236",
            "listen_type": "raw",
            "transport_type": "wss",
            "tcp_remotes": ["wss://0.0.0.0:3443"],
            "udp_remotes": ["0.0.0.0:5201"]
        },
        {
            "listen": "127.0.0.1:1237",
            "listen_type": "raw",
            "transport_type": "mwss",
            "tcp_remotes": ["wss://0.0.0.0:4443"],
            "udp_remotes": ["0.0.0.0:5201"]
        },
        {
            "listen": "127.0.0.1:1238",
            "listen_type": "raw",
            "transport_type": "mtcp",
            "tcp_remotes": ["0.0.0.0:4444"],
            "udp_remotes": ["0.0.0.0:5201"]
        },
        {
            "listen": "127.0.0.1:2443",
            "listen_type": "ws",
            "transport_type": "raw",
            "tcp_remotes": ["0.0.0.0:5201"],
            "udp_remotes": []
        },
        {
            "listen": "127.0.0.1:3443",
            "listen_type": "wss",
            "transport_type": "raw",
            "tcp_remotes": ["0.0.0.0:5201"],
            "udp_remotes": []
        },
        {
            "listen": "127.0.0.1:4443",
            "listen_type": "mwss",
            "transport_type": "raw",
            "tcp_remotes": ["0.0.0.0:5201"],
            "udp_remotes": []
        },
        {
            "listen": "127.0.0.1:4444",
            "listen_type": "mtcp",
            "transport_type": "raw",
            "tcp_remotes": ["0.0.0.0:5201"],
            "udp_remotes": []
        },
        {
            "label": "ping_test",
            "listen": "127.0.0.1:8888",
            "listen_type": "raw",
            "transport_type": "raw",
            "tcp_remotes": ["8.8.8.8:5201", "google.com:5201"]
        }
    ]
}
```

-   更多配置可以参考 [example](https://github.com/Ehco1996/ehco/tree/master/examples)

## 热重载配置

-   大于 1.1.0 版本的 ehco 支持热重载配置
-   通过 `kill -HUP pid` 信号来热重载配置
-   通过配置 `reload_interval` 来指定配置文件的热重载间隔
-   通过访问 POST `http://web_host:web_port/reload/` 接口来热重载配置
