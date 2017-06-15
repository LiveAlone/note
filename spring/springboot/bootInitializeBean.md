# initialize 初始化Bean 的配置方式

## initialize 初始化 bean

- ApplicationContextInitializer initialize 对应的实现配置Class (对应的启动Bean 配置信息)

```java
DelegatingApplicationContextInitializer
ContextIdApplicationContextInitializer
ConfigurationWarningsApplicationContextInitializer
ServerPortInfoApplicationContextInitializer
SharedMetadataReaderFactoryContextInitializer
AutoConfigurationReportLoggingInitializer
```

- 执行时间， 在Envoirment 初始化完成， prepareContext 环境初始化的时候， 执行Initlizer， 初始化启动配置信息。

## initializer 初始化Bean

- DelegatingApplicationContextInitializer 代理  env 指定的 "Context.initialzer.classes" 配置类。
  > 初始化 用户指定的ClassName， instance, 执行对应的 Initializer 初始化函数
- ContextIdApplicationContextInitializer 设置ApplicationId 启动ID 名称
  - 创建Id 的properties, spring.application.name, vcap.application.name, spring.config.name
  - 对应的后缀的添加: spring.application.index,vcap.application.instance_index,PORT 后缀
- ConfigurationWarningsApplicationContextInitializer 添加BeanFactoryProcessor
- ServerPortInfoApplicationContextInitializer 添加ServerPort 的监听， 当前Initlizer 监听 EmbeddedServletContainerInitializedEvent 内部容器启动事件。
    - 获取容器的 Context namespace 名称, env 添加该容器 对应的端口号配置方式。
- SharedMetadataReaderFactoryContextInitializer beanFactory 设置 CachingMetadataReaderFactoryPostProcessor, 缓存 Bean Metadata 数据信息。
- AutoConfigurationReportLoggingInitializer
  - 添加日志AutoConfigurationReportListener 创建配置信息， 记录Env, Context 的日志配置信息
  - 获取 BeanFactory 生成Report, 记录日志
