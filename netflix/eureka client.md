## eureka client 的配置方式

- application client 应用客户端配置方式
    ```
    <dependency>
        <groupId>com.netflix.eureka</groupId>
        <artifactId>eureka-client</artifactId>
        <version>1.1.16</version>
    </dependency>
    ```
- 客户端配置文件 eureka-client-{test,prod}.properties 不同的环境配置方式
    ```
     Application Name (eureka.name)
     Application Port (eureka.port)
     Virtual HostName (eureka.vipAddress)
     Eureka Service Urls (eureka.serviceUrls)
    ```

## eureka server 配置方式

- 环境需求
    + jdk1.8 higher
    + tomcat 的配置方式 6.0 
- Eureka Server Client 的 服务客户端的配置方式
    不同 properties 的属性配置方式

## eureka 构建配置方式

- install git
- clone 配置方式
- install build 构建方式
- 构建war, client-jar 构建的方式。

 
