# ribbon config

## RestTemplate Ribbon 关联

- @LoadBalanced 注解, 添加 RestTemplate, 通过 LoadBalancerClient 配置
- LoadBalancerClient 实现 ServiceInstanceChooser （spring cloud loadbalance, 定义Client 的LB）
  - choose(serviceId) 获取ServiceInstacne 获取服务实例
  - execute 执行 serviceId, 或指定ServiceInstance
  - reconstructURI 重新构建URL. (替换 ServierId 成 host:port 的格式)
- ```org.springframework.cloud.client.loadbalancer``` 包下

![lb](http://blog.didispace.com/assets/ribbon-code-1.png)

- LoadBalancerAutoConfiguration (spring LB 配置添加)
  - ```LoadBalancerInterceptor``` 请求 LB 拦截器 (LBClient, LBRequestFactory)
  - ```RestTemplateCustomizer``` RestTemplate 添加 Intecepter
  - @LoadBalanced 注解, 添加拦截器
- LoadBalancerInterceptor 拦截 request 请求, 通过 ```LoadBalancerClient```(ribbon 等 不同负载均衡的实现)
- ```org.springframework.cloud.netflix.ribbon.RibbonLoadBalancerClient``` 实现
  - getServer(ILoadBalance), ILoadBalancer接口中定义的chooseServer函数（并没有使用 父Class 的Choose Server）
- ```com.netflix.loadbalancer.ILoadBalancer``` Ribbon 的 均衡
  - 添加Server
  - chooseServer 均衡选择服务
  - markServerDown
  - getAllServers getReachableServers, 获取不同状态的服务

  ![ILB](http://blog.didispace.com/assets/ribbon-code-2.png)
  - BaseLoadBalancer类实现了基础的负载均衡, DynamicServerListLoadBalancer和ZoneAwareLoadBalancer 负载均衡策略
- spring ribbon client 结合的配置 RibbonClientConfiguration
  - 默认 ZoneAwareLoadBalancer, chooseServer 获取负载均衡的Server
- ```LoadBalancerInterceptor``` 拦截request 请求
  - 通过 ILB, 获取封装 RibbonServer
  - ```T returnVal = request.apply(serviceInstance);``` 通过 LoadBalancerRequest 发送对应服务请求
- ```ServerInstance``` 服务实例, 对应Host Name, 实现 RibbonServer 实现
- ServiceRequestWrapper 继承了 HttpRequestWrapper, 获取Url
  - 重写了 getUrl 的函数, 通过Balance
- ClientHttpRequestExecution 具体执行 ```execution.execute(serviceRequest, body)```
- SpringClientFactory 和 RibbonLoadBalancerContext
  - SpringClientFactory 创建客户端负载均衡工厂Class, 不同的Ribbon, 生成 Spring Context
  - RibbonLoadBalancerContext, 继承 LoadBalanceContext, 负载均衡上下文内容
    - reconstructURIWithServer, 通过Server 获取执行Url
- summary
  - LoadBalancerInterceptor 拦截器对RestTemplate的请求进行拦截，并利用Spring Cloud的负载均衡器LoadBalancerClient将以逻辑服务名为host的URI转换成具体的服务实例的过程。同时通过分析LoadBalancerClient的Ribbon实现RibbonLoadBalancerClient，可以知道在使用Ribbon实现负载均衡器的时候，实际使用的还是Ribbon中定义的ILoadBalancer接口的实现，自动化配置会采用ZoneAwareLoadBalancer的实例来进行客户端负载均衡实现

## 负载均衡器 (ILoadBalance 不同实现方式)

- AbstractLoadBalancer 负载均衡抽象
- BaseLoadBalancer 基础Server 定义
  - AllServerList, UpServerList server list 维护
  - LoadBalancerStats LB 对应 Server Status 维护
  - IPing 检测服务正常
  - IPingStrategy Server ping 策略, SerialPingStrategy 默认Ping 遍历循环策略.
    - IPingStrategy 实现自定义 ```pingServers(IPing ping, Server[] servers)``` 方式
  - IRule BaseLoadBalancer 方式 代理选择Server
- ```DynamicServerListLoadBalancer``` 服务实例清单动态更新
  - ServerList ```getInitialListOfServers``` 获取初始化的服务实例清单, ```getUpdatedListOfServers``` 获取更新的服务实例清单.
  ![serverlist](http://blog.didispace.com/assets/ribbon-code-3.png)
- ServerListUpdater 服务清单, DynamicServerListLoadBalancer
    - eureka server list 更新定义
- ServerListUpdater 定义服务器更新
  - PollingServerListUpdater：动态服务列表更新的默认策略，也就是说DynamicServerListLoadBalancer负载均衡器中的默认实现就是它，它通过定时任务的方式进行服务列表的更新。
    - initialDelayMs和refreshIntervalMs 设置启动延迟， 周期执行时间
  - EurekaNotificationServerListUpdater：该更新器也可服务于DynamicServerListLoadBalancer负载均衡器，但是它的触发机制与PollingServerListUpdater不同，它需要利用Eureka的事件监听器来驱动服务列表的更新操作。
  ![server list](http://blog.didispace.com/assets/ribbon-code-4.png)

  - ServerListFilter 服务列表的过滤
  ![server list filter](http://blog.didispace.com/assets/ribbon-code-6.png)
  - AbstractServerListFilter 过滤器 依据对象 LoadBalancerStats
  - ZoneAffinityServerListFilter 区域方法过滤, 服务区域， 消费区域 比较
    - 通过 shouldEnableZoneAffinity 判断 server 是否可用
      - 基础指标（实例数量、断路器断开数、活动请求数、实例平均负载）, 一个不符合条件， 不会进入Server. 保证
      - blackOutServerPercentage：故障实例百分比（断路器断开数 / 实例数量） >= 0.8
      - activeReqeustsPerServer：实例平均负载 >= 0.6
      - availableServers：可用实例数（实例数量 - 断路器断开数） < 2
  - DefaultNIWSServerListFilter 默认 NIWS（netflix internal web service） 过滤
  - ServerListSubsetFilter 大规模集群服务, 区域子集列表, 比较通信失败数量， 并发数量， 判断服务健康，剔除不健康
    - 区域感知过滤结果
    - 剔除不健康的实例
      1. 服务实例的并发连接数超过客户端配置的值
      1. 服务实例的失败数超过客户端配置的值
      1. 符合上面任一规则的服务实例剔除后，剔除比例小于客户端默认配置的百分比，默认为0.1（10%）
      1. 完成剔除后，清单已经少了至少10%（默认值）的服务实例，最后通过随机的方式从候选清单中选出一批实例加入到清单中，以保持服务实例子集与原来的数量一致，而默认的实例子集数量为20
  - ZonePreferenceServerListFilter 默认 Eureka Ribbon 过滤器， 区域过滤
- ZoneAwareLoadBalancer
  - ZoneAwareLoadBalancer 扩展 DynamicServerListLoadBalancer
  - DynamicServerListLoadBalancer 使用 RoundRobinRule 通过轮询,
  - 重写 setServerListForZones, 方法的重写

## 负载均衡策略 IRule 实现

![IRule](http://blog.didispace.com/assets/ribbon-code-5.png)

- AbstractLoadBalancerRule ILoadBalancer 均衡策略
- RandomRule server 通过Random 随机Server
- RetryRule ```maxRetryMillis``` 重试时间
- WeightedResponseTimeRule 通过权重 挑选实例
  - WeightedResponseTimeRule ```serverWeightTimer.schedule(new DynamicServerWeightTask(), 0, serverWeightTaskTimerInterval)``` 30 秒执行一次权重计算
  - 权重计算, 通过相应时间， 时间宽度， 宽度越大， 被访问距离越大
- BestAvailableRule 选出一个最空闲的实例
- AvailabilityFilteringRule 先过滤清单，再轮询选择
- ZoneAvoidanceRule zone 区域策略，静态函数策略
