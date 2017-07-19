# spring cloud common

## 基础的概念规范的定义

### basic

- org.springframework.cloud.client.ServiceInstance
  - 定义具体Service的 Host, port 访问
- org.springframework.cloud.client.serviceregistry.ServiceRegistry
  - 服务的注册, 解开注册的配置方式
  - 通过 ServiceRegistry 方式，获取 ServiceId

### acturator 服务监控

- 兼容 Spring acturator 的配置， 获取服务的运行 Feature

### circuit break 防止微服务 雪崩的 情况发生

- @EnableCircuitBreaker 启动 循环调用失败的情况

### loadbalance 负载均衡的配置方式

- @LoadBalance 注解添加 RestTemplate, 提供 http 负载均衡的配置方式·
- ServiceInstanceChooser 通过 ServiceId 获取 具体 ServiceInstance， 提供远程调用
- LoadBalancerClient LoadBalance 复杂均衡的Client 执行器
  - LoadBalancerRequest 执行 Service Instance, 远程获取结果信息
- RestTemplateCustomizer RestTemplate 
- LoadBalancerRequestTransformer load balance 转换器，微服务，添加负载均衡的信息
- LoadBalancedRetryPolicy 负载均衡的 重试 执行 策略配置方式

### hypermedia 中间件的 media 配置

### discovery 服务，注册发现的中间配置

- DiscoveryClient
  -  获取 所有 ServiceId，获取不同 ServiceId 的 ServiceInstance 配置方式
- @EnableDiscoveryClient 启动服务 Client 配置
- event
  - 心跳 检测 Event 事件类型
- health 
  - DiscoveryHealthIndicator 健康状态

