# 监控 ehco

## 指标 (Metrics)

启用 ehco 的 Web 功能后，您将能够访问一系列内置指标，用于监控和分析应用性能。

### 可用指标

-   **Node Exporter Metrics**: ehco 内嵌的 Node Exporter 提供的所有指标。
-   **ehco Metrics**: ehco 特有的指标，包括但不限于连接数和流量。更多细节请参考 [metrics.go](https://github.com/Ehco1996/ehco/blob/master/internal/metrics/metrics.go)。

## Prometheus 配置

要通过 Prometheus 监控 Ehco，您可以使用以下简单的配置作为起点。

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

此配置设定了 Prometheus 的抓取间隔、超时时间以及评估间隔，并定义了一个名为 "relay_nodes" 的作业，用于从 ehco 应用收集指标。

## Grafana 仪表板

为了在 Grafana 中可视化 ehco 的监控数据，您可以使用我们提供的仪表板模板。

-   下载并导入 [grafana.json](https://github.com/Ehco1996/ehco/blob/master/monitor/dashboard.json) 文件以设置 ehco 监控仪表板。

### 仪表板特性

-   **延迟监控**: 类似于 Smoking Ping 的延迟监控，帮助您追踪和分析网络延迟。

![延迟监控](/assets/monitor/ping.png)

-   **流量监控**: 实时查看通过 ehco 转发的流量。

![流量监控](/assets/monitor/traffic.png)

---

通过以上步骤，您可以有效地监控 ehco 应用的性能和状态。如果有任何问题或需要进一步的帮助，请不要犹豫，随时提问。
