# zuul doc

- routing 路由配置
- 动态路由， 监控， resiliency 重试， security
- 边缘 服务 edge service 使用

## zuul 提供

- Authentication and Security
- Insights and Monitoring
- Load Shedding
- Static Response handling
- Multiregion Resiliency

## works

- filter 执行特点
  - Type 定义routing flow 的Stage 状态
  - 执行顺序
  - criteria 定义执行条件
  - action 条件满足执行 action
- filter 不相互依赖， 通过Context 数据的依赖
- Filter 通过JVM 支持语言配置，周期校验变化，动态编译， 动态的改变Filter 的配置方式

- Filter Types
  - pre route to Origin 之前， request authentication, 选择服务， debug 日志
  - ROUTING filter 处理 reqeust，一般HTTP Request 请求
  - POST 发送request 到Origin (1 添加标准 headers, response 头信息, 2获取Statistics metrics 3 streaming response from origin）
  - ERROR filter 过滤其中错误
- 允许用户自定义 Filter
- 数据流程

![zuul](https://camo.githubusercontent.com/4eb7754152028cdebd5c09d1c6f5acc7683f0094/687474703a2f2f6e6574666c69782e6769746875622e696f2f7a75756c2f696d616765732f7a75756c2d726571756573742d6c6966656379636c652e706e67)

## zuul use in netflix

![use in netflix](https://camo.githubusercontent.com/5e596c573110bffb608614a09c97611107205d0d/687474703a2f2f6e6574666c69782e6769746875622e696f2f7a75756c2f696d616765732f7a75756c2d706879736963616c2d617263682e706e67)

- zuul 提供可扩展， 使用Component
  - [Hystrix](https://github.com/Netflix/Hystrix) wrap call to origin
  - [ribbon](https://github.com/Netflix/ribbon) 提供Info 网络新能
  - [Turbine](https://github.com/Netflix/Turbine) 细粒度分析 metrics, 快速的问题发现
  - [archaius](https://github.com/Netflix/archaius) 提供动态属性变化

## Surgical Routing

创建过滤路由，用户自定义需求， debuging 日志Cluster. 创建 Filter 记录Logger,

## stress Testing

- Archaius 动态配置，设置 zuul 去Small 集群，测试容量， 性能等