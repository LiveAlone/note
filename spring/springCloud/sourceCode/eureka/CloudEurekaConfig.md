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
- ```com.netflix.discovery.endpoint.EndpointUtils.getServiceUrlsFromConfig``` 获取Region(对应物理区域), Zone(相同区域,不同机房). 对应的ServiceUrl, 用来注册Service(applicationName)
  - 获取方式
    - 获取Region
    - 通过Region, 获取Zones
    - LinkedList, 希望zone在前, 获取不同区域的Zone 的ServiceUrl。 EurekaClientConfig 获取Zone 的ServiceUrl
      - zone 获取 service-url, Null 条件, 获取 ```defaultZone: http://localhost:8761/eureka/```
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
  - InstanceInfoReplicator.run() 中 ```discoveryClient.register()``` 发起注册请求
  - 通过REST APP 方式 请求注册, 参数 InstanceInfo 对应线程注册信息
- 服务的获取, 服务的续约 ```initScheduledTasks```
  - 服务的获取 Schedule 获取 远程 可用 服务 信息
    - Client 相关续约的判定
      - ```if (clientConfig.shouldFetchRegistry())``` 参数 ```eureka.client.fetch-registry=true```
      - registryFetchIntervalSeconds 配置 ```eureka.client.registry-fetch-interval-seconds=30```
    - 服务端 相关续约的判定 判断
      - eureka.instance.lease-renewal-interval-in-seconds=30 (client 端 定时请求 renew)
      - eureka.instance.lease-expiration-duration-in-seconds=90 (server 端 默认过期时间)
  - 服务的续约, heartbeat 检测, 服务可用防止被踢出
  - renew() 更新

## 服务注册中心

- Eureka Server ```com.netflix.eureka.resources``` 各类的 REST 请求的定义
  - 校验后 Register, ```org.springframework.cloud.netflix.eureka.server.InstanceRegistry``` register 注册 InstanceInfo
    - 继承PeerRegister, AbstractRetgister,注册服务,同步节点
  - publish Event 事件函数, 散播出去
  - ```com.netflix.eureka.registry.AbstractInstanceRegistry``` 发送 ```ConcurrentHashMap<String, Map<String, Lease<InstanceInfo>>>```
    - key: 对应服务名称。 value 对应InstanceId, InstanceInfo 的配置信息
- eureka client 对应的 Spring.factory 的配置添加

## eureka config 的配置文件

![eureka 调用图](http://nobodyiam.com/images/2016-06-25/architecture-detail.png)

### eureka server 实现方式

- Register (租测 InstanaceInfo 同时 Peer Node 的节点信息注册)
  - ```ApplicationResource``` Http 请求服务，```PeerAwareInstanceRegistryImpl``` 的 register 方法
  - ```PeerAwareInstanceRegistryImpl``` 调用 ```replicateToPeer``` 同步不同节点的 信息
  ![调用节点](http://nobodyiam.com/images/2016-06-25/eureka-server-register.png) 
- Renew 服务续约 心跳检测 服务的可用性

![服务节点](http://nobodyiam.com/images/2016-06-25/eureka-server-renew.png)

- Fetch Registers 服务端 获取节点信息

![节点信息获取](http://nobodyiam.com/images/2016-06-25/eureka-server-fetch.png)

- eviction 服务剔除 90 秒 Renew 的 自动剔除方式
  - ```eureka.instance.leaseExpirationDurationInSeconds``` 剔除配置
  - ```eureka.server.evictionIntervalTimerInMs``` 定期扫描
  ![剔除](http://nobodyiam.com/images/2016-06-25/eureka-server-evict.png)
- How Peer Replicates
  - Post 对应的 InstanceInfo
  - replication 不同节点 Instance
- How Peer Nodes are Discovered
  - EurekaClientConfig.getEurekaServerServiceUrls 获取所有的对等节点
  - eureka.server.peerEurekaNodesUpdateIntervalMs 获取 更新频率
  - 通过覆盖 getEurekaServerServiceUrls 方法， 实现可以 通过 DB 等读取
  ![](http://nobodyiam.com/images/2016-06-25/eureka-server-peer-discovery.png)
- How New Peer Initializes
  - server 重启 启动加入
  - 通过 Register，isReplication=true 完成
  ![](http://nobodyiam.com/images/2016-06-25/eureka-server-peer-init.png)

### Service Provider 服务提供

- Register ```eureka.client.registerWithEureka=true```
  ![](http://nobodyiam.com/images/2016-06-25/service-provider-register.png)
- Renew 定时 更新, 注意 这个是 Client 周期向服务端发送的请求
  - ```eureka.instance.leaseRenewalIntervalInSeconds``` 默认 30s
  - ```eureka.instance.leaseExpirationDurationInSeconds``` 服务失效时间 90s
  ![](http://nobodyiam.com/images/2016-06-25/service-provider-renew.png)

- Cancel @PreDestory 

![](http://nobodyiam.com/images/2016-06-25/service-provider-cancel.png)

- How Eureka Servers are Discovered
  - 过override getEurekaServerServiceUrls方法来提供自己的实现。定期更新频率可以通过eureka.client.eurekaServiceUrlPollIntervalSeconds配置
  ![](http://nobodyiam.com/images/2016-06-25/client-discover-eureka-server.png)

### Service Comsumer 实现

- Fetch Service Registries
  - ```eureka.client.shouldFetchRegistry=true``` 获取服务列表 缓存本地
  ![](http://nobodyiam.com/images/2016-06-25/service-consumer-fetch-registries.png)
- Update Service Registries
  - ```eureka.client.registryFetchIntervalSeconds``` 定时更新 
  ![](http://nobodyiam.com/images/2016-06-25/service-consumer-update-registries.png)
- How Eureka Servers are Discovered
  - 服务发现一样，通过 Db 等中间件
