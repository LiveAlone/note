# Cloud eureka

## 注册EurekaServer

- ```@EnableDiscoveryClient``` 配置注解
- 配置 ```eureka.client.serviceUrl.defaultZone``` 添加服务中心
- Annotation 启动 DiscoveryClient 配置
  - 继承配置关系
  ![Discovery Client](http://blog.didispace.com/assets/eureka-code-1.png)
  - spring common ```org.springframework.cloud.client.discovery.DiscoveryClient``` 定义服务发现接口
    - 获取 所有 serviceIds, 获取ServiceInstance, 端口 https 等配置
    - ```org.springframework.cloud.netflix.eureka.EurekaDiscoveryClient``` cloud-eureka-starter 服务获取的实现方式
      - 对Eureka 的依赖， EurekaClient eureka 包的依赖
      - 继承 LookService --> DiscoveryClient(netflix 中的实现), 继承eureka 的服务发现
    - netflix DiscoveryClient 使用
      - 这个类用于帮助与Eureka Server互相协作。
        - Eureka Client负责了下面的任务
          - 向Eureka Server注册服务实例
          - 向Eureka Server为租约续期
          - 当服务关闭期间，向Eureka Server取消租约
          - 查询Eureka Server中的服务实例列表
        - Eureka Client还需要配置一个Eureka Server的URL列表。
- ```com.netflix.discovery.endpoint.EndpointUtils``` 获取Region, Zone.
  - 获取Region, 一个微服务对应一个 Region, 默认 Default, ```eureka.client.region``` 定义
  - getAvailabilityZones 多个 zone, ```eureka.client.serviceUrl.defaultZone```, ```eureka.client.availability-zones``` region zone 一对多的关系
  - 通过```,``` 分割符号
- ```EurekaClientConfigBean``` 通过Zone获取 ServiceUrls 的配置链接
  - EurekaClientConfig, EurekaConstants eureka 的配置信息
- 总结
  - Zone, Region 的访问逻辑, 优先访问同自己一个Zone中的实例，其次才访问其他Zone中的实例。通过Region和Zone的两层级别定义，配合实际部署的物理结构，我们就可以有效的设计出区域性故障的容错集群

## 服务注册

- DiscoveryClient initScheduledTasks 启动注册服务的心跳检测
  - ```if (clientConfig.shouldRegisterWithEureka())``` 判定服务注册
  - InstanceInfoReplicator.run() 中 ```discoveryClient.register()``` 注册自己的服务
  - 通过REST APP 方式 请求注册, 参数 InstanceInfo 对应线程注册信息
- 服务的获取, 服务的续约 ```initScheduledTasks```
  - 服务的获取 Schedule 获取 远程 可用 服务 信息
    - Client 相关续约的判定
      - ```if (clientConfig.shouldFetchRegistry())``` 参数 ```eureka.client.fetch-registry=true```
      - registryFetchIntervalSeconds 配置 ```eureka.client.registry-fetch-interval-seconds=30```
    - 服务端 相关续约的判定
      - eureka.instance.lease-renewal-interval-in-seconds=30
      - eureka.instance.lease-expiration-duration-in-seconds=90
  - 服务的续约, heartbeat 检测, 服务可用防止被踢出
  - renew() 更新

## 服务注册中心

- Eureka Server ```com.netflix.eureka.resources``` 各类的 REST 请求的定义
  - 校验后 Register, ```org.springframework.cloud.netflix.eureka.server.InstanceRegistry``` register 注册 InstanceInfo
  - publish Event 事件函数, 散播出去
  - ```com.netflix.eureka.registry.AbstractInstanceRegistry``` 发送 ```ConcurrentHashMap<String, Map<String, Lease<InstanceInfo>>>```
    - key: 对应服务名称。 value 对应InstanceId, InstanceInfo 的配置信息
- eureka client 对应的 Spring.factory 的配置添加