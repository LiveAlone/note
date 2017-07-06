# Spring Cloud basic

## spring feature
- spring cloud 提供分布式（配置管理，服务发现，循环失败，智能路由, 微代理方式，控制总线）
- 提供功能
  - 分布式，版本 控制
  - 服务注册发现
  - 服务调用
  - 动态路由
  - 负载均衡
  - 回环调用失败
  - 分布式消息
- spring cloud 提供了 特殊的工具，特殊的服务
  - bootstap context
  - 刷新 Scopre
  - env endpoint 环境节点
- spring-cloud common 抽象的定义， 有 spring cloid netflix 和 spring cloud consul 的实现

## spring cloud config client 提供一些特殊的配置服务

### bootstap application context

- spring cloud app 创建 bootstap context 
- BootStapContext 就是 Parent 环境
- bootStap 处理 优先级 高， 配置 会被 Exteral 覆盖
- bootstap, 添加配置 ```spring.cloud.bootstrap.enabled=false``` bootstap process 启动配置

### spring client 继承关系

- cloud 创建 BootStrap Context 作为 Parent Context, 继承覆盖 Properties
- spring cloud 外部配置 Sources
  - bootstap context 获取 PropertySourceLocator， spring config server , 获取配置属性
  - classpath:bootstrap.yml 添加配置信息
- bootstrap 是 Parent， 可以认为默认 配置信息
- child application context 对应同名的 property source, clild 会生效
- application context 继承关系的 Context

### changing location bootstrap
- ```spring.cloud.bootstrap.name``` ```pring.cloud.bootstrap.location``` 配置 BootStrap ApplicationContext, 设置 Env
- 同样 Profile 配置， bootstrap-dev.properties ... 

### 从 远程 Properties, 覆盖 Overriding

- 配置 远程文件的配置， 是否可覆盖

### 客户定制化 Bootstrap Configuration

- 用户定制化 ```BootstrapConfiguration``` 添加 spring.factory 中
- 定制化 Property Source 的配置方式

### 用户定制化 Bootstrap Property Source 

- PropertySourceLocator 继承， @Configuration, 添加配置 Class
- spring.factory 添加 ```org.springframework.cloud.bootstrap.BootstrapConfiguration=sample.custom.CustomPropertySourceLocator``` 添加 External 配置文件信息。

### envoirment change 环境变量

- ```ApplicationListeners``` 监听  ```EnvironmentChangeEvent`` 
  - 重新绑定 @ConfigurationProperties, bean context
  - 设置 ```logging.level.*``` 
- 不建议， 循环的调用， 监听 Configuraion Server Env 变化， 使用 Spring Cloud Bus 的配置
- env 环境变化， ChangeEvent, Rebinding , 重新设置Properties

### Refresh Scopre 

- Refresh Scope 刷新， 方法，自定义的刷新方式
- bean 方式 定义刷新方式

### 加密， 解密方式

### endpoint
- 定义 /env /refresh /restart /pause /resume 方式

## spring cloud commons, 公共方法的抽象

### @EnableDiscoveryClient 来提供远程 Service 的服务发现

- spring.factory 定义 DiscoryClient 定义
- DiscoveryClient 对应接口实现
- 设置 autoRegister=false 服务不被发现

### ServiceRegistry
- configuration 通过 ServiceRegistry 注册 Registration

#### service registry endpoint  /service-registry
- 服务的注册发现

### Spring RestTemplate as a Load Balancer Client
- load balance 方式执行
