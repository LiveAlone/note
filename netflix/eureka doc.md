# eureka config doc

## AWS 服务发现, 负载均衡的 中间件。

- Eureka Clinet 简易的交互方式 通过 robin-loadbance 方式， 调用负载均衡的配置方式。
- 负载均衡： traffic, resource usage, error condition 等情况提供优秀的返回情况。

## 使用条件：

- AWS Server 不固定的状态， ip 变动比较大， 不固定的可知的条件下。
- eureka LB 服务发现的中间件。
> ELB (elastic load blance) 方式， 通过  边缘服务对应的 Web-user 的一种负载均衡的方式。

## 普通的负载均衡 区别：

- instance server host 级别的差异。 传统 Client 指导 所有的Server 节点的配置信息。
- eureka 提供 无 粘性的， client 不需要记录 每个Server 的状态信息 （address status 等配置信息）
- 客户端 不用关心 Server 集群的管理， 可用特性。等。
> （netflix 红黑发布 提出： 传统轮训的方式， 每台服务器的上线发布。风险 结束时 Server Error。 服务 拆分 两个集群， 通过LoadBalan 方式， 替换服务。）

## Eureka 的使用方式：

  1 红黑发布
  2 cassandra 服务缓存
  3 memcache 缓存， node 节点可用性。
  5 不同的节点提供不同的服务配置信息。

## eureka 的使用时间

- 希望微服务管理集群配置， memcache 不同的服务列表。
- 希望LB 微服务的治理，Client 未知 server 的执行信息情况。

## client server 交互方式

eureka 提供服务发现功能， 不限制 通信方式，可以通过 thrift, RPC， http(s) 等交互方式。

## eureka 的架构沟通方式

![Alt text](https://github.com/Netflix/eureka/raw/master/images/eureka_architecture.png "Optional title")

> sclient 通过 eureka 集群交互方式， 获取Service 的信息，通过RemoteCall方式 远程调用参数的配制信息
通过定时的心跳检测方式 判定 远程 Servie 处于运行的状态。

## non-java 的客户端配置方式

## 可配置特性

- 可以动态的移动 Node 节点。add remove 配置方式。
- 使用  [archaius](https://github.com/Netflix/archaius)  通过配置源动态的修改配置信息

## 集群高可用

## 不同的区域不可沟通的特性

## 监控

- 通过使用 [servo](https://github.com/Netflix/servo/wiki) 远程监控的方式。
- 监控性能， 警告， JMX regsiter 等Watch 信息。