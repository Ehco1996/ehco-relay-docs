# 监控

## Metrics

在开启了 web 功能后 ehco 内置了一些 metrics

-   内嵌的 node_exporter 的所有 metrics
-   ehco 的 metrics，包括连接数，流量等，更多请看代码： [metrics.go](https://github.com/Ehco1996/ehco/blob/master/internal/metrics/metrics.go)

## Prometheus

一个简单的 prometheus 配置文件如下

```yaml
global:
    scrape_interval: 15s
    scrape_timeout: 10s
    evaluation_interval: 1m
scrape_configs:
    - job_name: "relay_nodes"
      scrape_interval: "30s"
      metrics_path: "/metrics/"
      static_configs:
          - targets:
                - 127.0.0.1:9000
```

## Grafana

dashboard 的 json 文件在 [grafana.json](https://github.com/Ehco1996/ehco/blob/master/monitor/dashboard.json) 请下载后导入

-   类似 Smoking Ping 的延迟监控

<img src="/assets/monitor/ping.png">

-   流量监控

<img src="/assets/monitor/traffic.png">
