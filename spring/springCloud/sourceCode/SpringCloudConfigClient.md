# Spring Cloud Config client

## spring cloud native
- 提供 Spring-Common, Spring-Context 主要依赖
  - Spring-Context 提供（bootstrap context, encryption 加密，environment endpoints 监控节点）
  - Spring-Common 提供基础 抽象概念 （SpringCloudNetflix, SpringCloudConsul 的实现）
- 简历在 SpringBoot 基础上
  - 提供 Common Config
  - endPoint 公共的管理
  - 监控 Task

### spring bootstrap application context 
- 当前 ApplicationContext 创建 ParentApplicationContext, loading 外部的配置文件， 解密外部的配置文件
- 连个 Context 共享 Envoirment, sub application 有更高的权限。
- 通过 ``` spring.cloud.bootstrap.enabled=false ``` 启动关闭 spring.cloud 配置(ef: System Propreties 中添加)
- Application Context 的继承方式
  - 继承 Bootstrap Context， profile, property source, 
  - 额外的 Property Source 
    - "bootstrap", ```CompositePropertySource``` 有高的优先级, 如果有 ```PropertySourceLocators```
    - "applicationConfig" classpath:bootstrap.yml 由于 Parent , 优先级较低
  - 创建方式
    - SpringApplicationBuilder 创建 parent, child, sibling 的 Context
    - Note: ```SpringApplicationBuilder``` 中 Env 继承的 Context, 不是默认的。 兄弟 ApplicationContext 不必有相同的 Env 的 共享
- 修改 bootstrp 名称， ```spring.cloud.bootstrap.name```, ```spring.cloud.bootstrap.location```, 
  - bootstrap-(env).yml 的配置
- config server  远程配置
  - 远程配置不允许被覆盖， 如果像远程的 属性可以被覆盖
    - 远程 config server 设置 ```pring.cloud.config.allowOverride=true```
    - 本地设置 ```spring.cloud.config.overrideNone=true``` 允许远程配置
    - 只有 System properties, env vars,  ``` spring.cloud.config.overrideSystemProperties=false ``` 允许覆盖方式
- 定制化 Bootstrap Configuration
  - 在 spring.factory 添加 ```org.springframework.cloud.bootstrap.BootstrapConfiguration``` 添加 @Configuration 配置 Class
  - bootstrapConfiguration 不要添加 ComponentScan 通过 SpringBootApplication 添加
  - bootstrap 添加 spring.factory， @Beans, ApplicationContextInitializer 添加执行环境

### 定制化Bootstrap property sources

- 定制化 ```PropertySourceLocator``` 继承，通过 bootstrap process添加外部的配置. spring.factory 添加
```java
org.springframework.cloud.bootstrap.BootstrapConfiguration=sample.custom.CustomPropertySourceLocator
```

### Enviroment Changes 配置属性的改变

- 通过 监听 ```EnvironmentChangeEvent``` 通过 ```ApplicationListeners``` 监听。 Application 会
    1. 重新绑定 @ConfigurationProperties
    1. 设置 Log Level
- env 不会默认拉取变化的配置。 @Schedule 定时， 或者通过 Spring Cloud Bus 方式获取配置
- EnvoirmentChangeEvent 修改 Env 的配置信息，对应Class 不会定时刷新
- @RefreshScope 定位 Bean, Env 改变时候， 刷新Bean
- RefreshScope， refreshAll() 函数，/refresh 来刷新， 默认 延迟加载刷新

### Encryption and Decryption
- environment  pre-processor 去 解密

### endpoints 
- /env, @ConfigurationProperties Envoirment
- /refresh @RefreshScope
- /restart ApplicationContext restart
- /pause /resume start或stop ApplicationContext

## spring cloud commons abstractions
- @EnableDiscoveryClient eureka, consul, zookeeper 服务发现
- ServiceRegistry
```java
@Configuration
@EnableDiscoveryClient(autoRegister=false)
public class MyConfiguration {
    private ServiceRegistry registry;

    public MyConfiguration(ServiceRegistry registry) {
        this.registry = registry;
    }

    // called via some external process, such as an event or a custom actuator endpoint
    public void register() {
        Registration registration = constructRegistration();
        this.registry.register(registration);
    }
}
```
- ServiceRegistry Auto-Registration ```@EnableDiscoveryClient(autoRegister=false)``` 自动注册的配置
- Spring RestTemplate as a Load Balancer Client
```java
@Configuration
public class MyConfiguration {

    @LoadBalanced
    @Bean
    RestTemplate restTemplate() {
        return new RestTemplate();
    }
}

public class MyClass {
    @Autowired
    private RestTemplate restTemplate;

    public String doOtherStuff() {
        String results = restTemplate.getForObject("http://stores/stores", String.class);
        return results;
    }
}
```
- Retrying Failed Requests
  - ```spring.cloud.loadbalancer.retry.enabled=false``` 通过 feigin, ribbon 配置
- Ignore Network Interfaces
  - 对应的 网络 前缀， 忽略 Service 发现 注册
  - 可以设置 默认本地注册方式