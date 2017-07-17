# spring cloud config

- EnableCircuitBreaker
  - 启动 断路器，远程调用多次失败以后，返回错误信息。
- org.springframework.cloud.client.discovery.DiscoveryClient
  - 启动注解 @EnableDiscoveryClient
  - 从 eureka, consul 发现注册的服务
- LoadBalance 不同 Lb 的配置策略
- service register 服务注册
  - contract service 注册对应的服务