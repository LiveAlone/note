# spring actuator 监控添加方式

## 基础监控查询

1. /env/{key} 获取不同的Properties 的属性
1. /configprops 不同的配置对象
1. /autoconfig 获取 自动匹配 Match 的配置信息。
1. /beans 获取Spring 对应Bean 的注册内容。
1. /info 所有的配置 info. 的配置开头， 会显示。
1. /health 校验 Application 健康状态
1. /metrics 显示子系统的 管理信息。

## spring actuator 文档Content

- actuator 提供的监控点
  - actuator 提供下面不同的 Path 监控不同的配置内容
  - auditevents 当前App 的 events
  - autoconfig 当前AutoConfig 以及配置的原因。
  - beans app 中 RegisterBean 的配置信息
  - configprops app 对应的 @ConfigurationProperties 对应的配置情况。
  - dump 当前的APP dump 线程的内容
  - env 对应的 ConfigEnvoirment 的内容。
  - flyway  flyway database migrations
  - health 权限开通， 获取App 的情况。
  - logger 日志配置信息
  - liquibase database 基础配置信息
  - mappings 对应的 Controller 映射关系。
  - shutdown  app 停止， 默认不允许
  - trace 100 Http 相关信息
- 如果使用的SpringMVC
  - doc 获取 actuator doc 相关配置
  - heapdump gzip
  - jolokia JMX 通过Http 的方式。
  - logfile 对应日志内容。

## health customer 实现方式

- 不同的 Cassandra, Disk, Elasticsearch 对应的实现了 AbstractHealthIndicator。 提供了Health 的监控方式。
- 自定义的 HealthIndicator 实现 HealthIndicator 接口即可。
- health 默认非暴露的```endpoints.health.sensitive```
- Health 不同Status Spring 定义```FATAL, DOWN, OUT_OF_SERVICE, UNKNOWN, UP```, Http 请求返回不同的 Httped 状态。

## InfoContributor暴露spring APP 对应的（Info 配置信息）

- InfoContributors 自动配置App 的 Properties 属性。
  - EnvironmentInfoContributor info. 的配置属性。
  - GitInfoContributor git.propreties 的属性配置。
  - BuildInfoContributor META-INF/build-info.properties 配置信息。

## sensitive endpoint 通过 Actuator 用户限制， Security(通过Spring Security 监控状态信息)

- ```management.context-path``` Path 前缀添加
- ```management.port``` port
- ```management.security.enabled``` 关闭Security 的配置
- 配置SSL 指定Manager 默认不同的SSL 实现方式

## 通过JMX 监控管理 JMX消息配置信息

- todo

## remote shell 配置

- 依赖 crashub package, ssh 方式远程查看
- 启动生成 64 ssh 密码登录校验。

## 不同性能监控

- metrics 不同的类型
  - system metrics 内存 JVM 的相关信息。 Class Loader, Garbage 垃圾回收相关信息
  - DatasourceMetrics 数据源 衡量定义方式
  - Cache Metrics 对应缓存 size 衡量
  - Tomcat Metrics 配置
- metrics 可以导入不同的DB中， Redis 导入对应的 Metrics 配置信息
