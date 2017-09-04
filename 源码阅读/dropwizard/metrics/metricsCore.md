# Metrics Core

## metrics 配置方式

- metric register 注册监控
- metric 类型 gauges, counter, histograms, meters, timers

## health check 校验健康状态

- Health check register 注册监控

## report 不同监听防护 (不同的监控系统report, spring health 通过接口暴露的方式)

- console
- cvs
- self4j
- Ganglia 监控系统
- Graphite

## 插件监听不同的 Source

- apache httpClient
- jersey 配置
- jetty
- jetty 监控
- log4j log4j2 配置
- jvm

## ryantenney metrics-spring 集成的配置方式

- 集成 metrics-spring 的配置方式