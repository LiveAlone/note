## spring factory Listener 启动配置方式

> spring boot 启动通过Listener 监听启动的配置

## 加载过程
  
- SpringApplicationRunListeners spring boot 启动监控启动配置。
- SpringApplicationRunListener boot App 启动容器监控配置。
    - EventPublishingRunListener 唯一的 Spring 容器启动事件监听
    - 当前的 SpringApplication 获取对应的 ApplicationListener app 的容器监听器。
- ApplicationListener spring.factory 启动的app 监听配置，
- ApplicationContextInitializer springApplication 启动初始化配置器。

## Spring bott 初始化 Initlizer Bean 的初始化配置方式 spring.factory

- SpringBoot package 下
```java
org.springframework.boot.context.ConfigurationWarningsApplicationContextInitializer
org.springframework.boot.context.ContextIdApplicationContextInitializer
org.springframework.boot.context.config.DelegatingApplicationContextInitializer
org.springframework.boot.context.embedded.ServerPortInfoApplicationContextInitializer
```
- spring auto congfig 添加Initlizer
```java
org.springframework.boot.autoconfigure.SharedMetadataReaderFactoryContextInitializer
org.springframework.boot.autoconfigure.logging.AutoConfigurationReportLoggingInitializer
```

## Spring Boot 初始化 容器的监听配置

- spring boot bean.factory 监听器(spring boot, spring autoconfig 配置)
```java
org.springframework.boot.ClearCachesApplicationListener
org.springframework.boot.builder.ParentContextCloserApplicationListener
org.springframework.boot.context.FileEncodingApplicationListener
org.springframework.boot.context.config.AnsiOutputApplicationListener
org.springframework.boot.context.config.ConfigFileApplicationListener
org.springframework.boot.context.config.DelegatingApplicationListener
org.springframework.boot.liquibase.LiquibaseServiceLocatorApplicationListener
org.springframework.boot.logging.ClasspathLoggingApplicationListener
org.springframework.boot.logging.LoggingApplicationListener
```
- spring autoconfig 添加 Listener
```java
org.springframework.boot.autoconfigure.BackgroundPreinitializer
```

## Spring Listener 监听的实现
- ClearCachesApplicationListener
    - 监听事件 ContextRefreshedEvent，事件本身没有特殊定义
    - ReflectUtil 清除Cache 缓存配置信息。
    - 清空当前Thread ClassLoaderContext 的配置信息。
- ParentContextCloserApplicationListener 
    - 监听 ParentContextAvailableEvent 事件
    - 监听ApplicationContext  父类 ApplicationContext 已经关闭的状态。
- FileEncodingApplicationListener 文件编码配置监听
    - 监听 ApplicationEnvironmentPreparedEvent 优先级最低
    - 对比当前系统的 File Encoding, 对应 resolver 的Encoding ，不相同 异常信息产生。
- AnsiOutputApplicationListener ansi 日志信息输出
    - 监听事件 ApplicationEnvironmentPreparedEvent
    - spring.output.ansi. 对应Prefix 的配置信息， AnsiOutput 编码输出设置方式。
- ConfigFileApplicationListener 配置文件监听器， 监听两个事件
    1. ApplicationEnvironmentPreparedEvent 添加配置文件. 获取 EnvironmentPostProcessor 实现
        - CloudFoundryVcapEnvironmentPostProcessor springCloud 配置文件的 load 做的一些事情（！！！ eureka config 的配置）
        - SpringApplicationJsonEnvironmentPostProcessor spring json 获取对应的配置文件信息。 
          > spring boot MTF 对应的 map 类型的Json 配置方式， 读取对应的配置信息 到Envoirment 配置。

        - ConfigFileApplicationListener 也是一个 Envoirment Processor 
          1. 获取Env 不同的Profile 加载不同的配置文件的信息。
          2. env 中设置对应的 Ingre 忽略的Bean 的配置方式。
          3. 这个时候！！ Env 对应的初始化完成， 绑定对应的 Application 中。
    2. ApplicationPreparedEvent 对应Spring Boot 初始化运行的事件。 添加 post-processor, post-configure 配置信息。
        - 添加 PropertySourceOrderingPostProcessor 对应的 Env 配置文件 执行的 排序方式。
        - BeanFactoryPostProcessor bean factory 用户自定义的 修改配置方式。
- DelegatingApplicationListener 获取环境中"context.listener.classes" 对应的Class,执行用户自定义的监听事件
- LiquibaseServiceLocatorApplicationListener 以后
- ClasspathLoggingApplicationListener 打印当前的Classpath 的路径信息
- LoggingApplicationListener 日志启动的监听初始化(TODO)

## spring 事件类型的获取 发放
- multiCast Event 通过Event 的事件类型判断对应的执行Listener 执行。
