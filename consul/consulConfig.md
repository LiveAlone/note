# Consul doc config
服务发现，简单的配置方式。
1. 服务发现: 简单的服务发现，服务注册。 注册 Sass 服务。
1. 失败检测: 服务健康检测，提供 circuit breaker 检测。
1. 多数据中心：监控数据中心的服务。
1. Key-Value 存储， 提供 动态配置，特点表示， 协调， leader 选举， 等。

### 基础架构
- 分布式， 高可用的系统
- 每个Node 提供一个 Service, 启动一个 Consul Agent. 
- 启动 Agent。 1 services 发现， 2 key-value 存储方式
- agent 通过 service 的 健康检测。
- agents 链接 一个或者多个  Consul Servers
  - consul server, 数据存储， 备份
  - 选举 Leader
  - consul server 3 - 5, 选举 Leader 防止数据丢失。
  - 通过 Consul Server, Consul Agent, 发现 Service

### 比较其他的软件
- zk, doozerd, 比较
  - 相同的分布式节点， 需要一定数量（3 - 5）, 提供强一致性（分布式网络）。
- eureka 的 比较
  - eureka 提供 弱一致性， 通过心跳检测方式。
  - Paxos 强一致性算法