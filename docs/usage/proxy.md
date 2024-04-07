# 代理集的前置代理

## 介绍

ehco 可以做为代理集的前置代理， 自动合并代理集中具有相同前缀的代理， 以达到负载均衡/叠加带宽的效果

目前支持的代理集有：

-   clash proxy provider

## 配置步骤

在 [配置节点](../node/manage.md) 添加节点时，在配置的末尾处配置好 Proxy provider 的名字和 url

<img src="/assets/node/add-proxy-provider.png">

-   name: 代理集的名字
-   url: 代理集的 url

该 url 返回的代理集必须是 clash 格式的，例子如下：

```yaml
proxies:
    - name: us-1
      server: s1
      password:
      port: 1
    - name: us-2
      server: s2
      port: 2
    - name: jb-1
      server: s3
      password: pass
      port: 3
```

## 重写代理集

访问 ehco 的 web 页面，可以看到重写后的代理集的信息,一个代理集会被重写成两个新的代理集，一个是原代理集的名字，另一个是原代理集的名字加上`-lb`

<img src="/assets/node/proxy-provider.png">

### name

打开连接，可以看到代理集的详细信息

```yaml
proxies:
    - name: us-1
      server: <ip-of-your-node>
      password:
      port: 1
    - name: us-2
      server: <ip-of-your-node>
      port: 2
    - name: jb-1
      server: <ip-of-your-node>
      password: pass
      port: 3
```

-   每个代理的 server 字段被替换为了节点的 IP 地址
-   其他字段不变

### name-lb

打开连接，可以看到代理集的详细信息

```yaml
proxies:
    - name: us-lb
      server: <ip-of-your-node>
      password:
      port: <new-port>
    - name: jp-lb
      server: <ip-of-your-node>
```

-   代理集中具有相同前缀的代理被合并为一个代理
-   合并后的代理的名称为前缀加上`-lb`
-   合并后的代理的 server 字段被替换为节点的 IP 地址
-   合并后的代理的 port 字段被替换为一个新的端口，这个端口是随机生成的，不会与其他代理的端口冲突

## 使用

拿到重写后的代理集 url 后，可以在 clash 中使用

```yaml
proxy-providers:
    nas:
        type: http
        url: http://192.168.31.30:20086/clash_proxy_provider/?sub_name=nas&grouped=true
        path: ./providers/nas.yaml
```

这样就可以在 clash 中使用代理集了，并且所有流量都会通过 ehco 节点转发

如果您配置了[监控功能](../usage/monitor.md)，可以在监控页面看到代理集的使用情况

<img src="/assets/node/proxy-traffic.png">
