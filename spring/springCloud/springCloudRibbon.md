# spring ribbon 的配置方式
Http RestFul 框架实现， ribbon + restTemplate 或 feign

ribbon 是一个负载均衡的服务， 控制 http, feign 使用 ribbon

## 实践
1. eureka 服务启动 
1. 通过 eureka 注册 service-hi 服务
1. 通过 RestTemplate 的配置方式 实现 LB。 （通过 eureka 获取不同的服务的配置信息）

不知道为什么 添加下面的配置信息。
```java
      <dependency>
            <groupId>org.apache.httpcomponents</groupId>
            <artifactId>httpclient</artifactId>
            <version>4.5.2</version>
        </dependency>
```
