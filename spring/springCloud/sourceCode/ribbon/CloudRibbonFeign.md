# cloud ribbon 配置

## Ribbon

- RibbonClientConfiguration 基础RibbonClient的配置Class
  - IClientConfig 默认实现 DefaultClientConfigImpl. Client 的配置, 负载均衡, 方法执行
  - IRule 定义 LoadBalance 使用规则 权重，随机轮训的方式
  - IPing ping pong
  - ILoadBalancer, 负载 Server 的均衡方式
  - ServerListUpdater, 修改Server 列表的配置信息
- ribbon 通过 eureka 集成执行, 或者指定ServerList 的执行列表

## feign 配置

- 通过 Ribbon 解析以来的注入方式