---
title: "🔥Prometeus指标"
linkTitle: "🔥Prometeus指标"
weight: 3
description:
---

nmid server内置prometheus监控服务，需要配置文件开启使用，默认指标数据nmid func调用次数(qps)，以及nmid func调用耗时(during)

## config
在配置文件夹中的server.yaml最后加入以下内容，当然配置参数根据自身情况进行修改

```yml
Prometheus:
  enable: true     #是否开启
  host: "0.0.0.0"
  port: "9081"
  path: "/metrics"
```

## Grafana展示 

### func qps

<img src="/images/qps.png" alt="nmid architecture"/>  

### func during

<img src="/images/during.png" alt="nmid architecture"/>  