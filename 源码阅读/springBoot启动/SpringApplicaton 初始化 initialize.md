# SpringApplicaton 初始化 initialize(source)

## spring.factories 定义不同的 ApplicationContextInitializer SpringContext 的回调接口：SpringApplication 定义了获取

### SpringBoot 包下：

- ConfigurationWarningsApplicationContextInitializer
  - 添加BeanFactoryProcessor 定义，warn 配置的信息错误。
- ContextIdApplicationContextInitializer
  - 设置ApplicationContext Id ,通过 "spring.application.name" , "vcap.application.name", "spring.config.name"
  - 设置Bean的 ApplicationId
- DelegatingApplicationContextInitializer
  - 获取其他 用户自定义的 Initializer 配置信息
  - context.initializer.classes 属性配置，
- ServerPortInfoApplicationContextInitializer
  - 添加Event Listener 方式， 监听端口号配置。
  - 设置默认的Port 监听位置， 监听端口号。

### springBoot AutoConfiguration 目录下：

- SharedMetadataReaderFactoryContextInitializer
  - 通过创建一个 CachingMetadataReaderFactory 在 SpringBoot, 和 ConfigurationClassPostProcessor
- AutoConfigurationReportLoggingInitializer
  - 回去配置日志文件的 启动方式

### initlize 执行时间

- spring application prepareContext 会执行不同的 Initlizer
- 通过 spring.factory 文件获取对应的Initlizer执行器

## 加载不同 spring.factories 下 ApplicationListener

### SpringBoot 数据包

- ClearCachesApplicationListener
  - ApplicationContext 加载过程， analyse Bean 会缓存 Method, field 等字段。
    - 清除 ReflectionUtils 的工具
    - 执行 当前线程 父线程的 ClearCache
- ParentContextCloserApplicationListener
  - 监听 当Parent 的Content 关闭， 子 ApplicationContext 也会关闭操作。
- FileEncodingApplicationListener 当 文件的编码 不匹配监听。
- AnsiOutputApplicationListener
  - spring.output.ansi 依赖配置。
  - 终端支持 ansi 支持彩色效果输出。
- ConfigFileApplicationListener
  - EnvironmentPostProcessor 的实现， 
  - 加载配置文件 ： 默认 application.properties application.yml
  - 加载位置 classpath: classpath:config, file: file:/config
  - 加载不同的 Profile 下的配置文件。
  - 制定： spring.config.location 配置文件的路径。
  - 通过SpringApplication 制定： properties 属性。
- DelegatingApplicationListener
  - 发现 配置下， context.listener.classes 其他的 ApplicationListener
- LiquibaseServiceLocatorApplicationListener
  - 外部数据 插件配置方式。
- ClasspathLoggingApplicationListener
  - log 记录当前 的Classpath 的配置
- LoggingApplicationListener
  - 配置日志系统， 当前的日志级别。

-------------------------------------------------------------------

## processor

- BeanFactoryPostProcessor 允许修改BeanDefinition配置
  - BeanFactoryPostProcessor 自动检测， BeanDefition 创建之前， 修改定义
  - demo: PropertyResourceConfigurer 添加定义方式
  - BeanFactoryPostProcessor 关注Bean Definition 的信息， 而不是 Bean Instance （想修改Instance， 需要 BeanPostProcesor 继承）
- BeanDefinitionRegistryPostProcessor 继承BeanFactoryPostProcessor
  - register furthe bean definition, before 继承BeanFactoryPostProcessor, 可以定义 BenFacotryPostProcessor

### beanFacotory Processor

- 添加位置
  - initlizer 添加Porcessor
    - ConfigurationWarningsPostProcessor 校验 definition 正确
    - CachingMetadataReaderFactoryPostProcessor, 添加AnnotationProcessor, 添加ReadFacotory 的配置信息。
  - listener 添加配置 PropertySourceOrderingPostProcessor
    - env 中不同的 Resource 资源的配置添加

### BeanPostProcessor

- ApplicationContextAwareProcessor 不同的Aware 注入定义
  - 同时 忽略Aware 的依赖注入
- ApplicationListenerDetector 获取 ApplicationListener 实现Bean
- WebApplicationContextServletContextAwareProcessor
- ConfigurationClassPostProcessor 处理 @Configuration 注解的Class, @Bean 的注册的Bean 解析

### PostProcessorRegistrationDelegate 执行BeanFactoryPostProcessor

- BeanFactoryProcessor 继承 register Processor,  before 预先执行
- BeanFactory 创建， BeanFactoryPostProcessor, BeanPostProcessor 定义。
- 执行ConfigurationClassPostProcessor, 加载对应的注册Class, 不同Config的Class
- register BeanPostProcessor 定义
- beanFactory 加载对应的Bean, no-lazy 启动, 这个时候 BeanPostProcessor 的已经定义添加了。
> debug 调试, 执行完成BeanFacotoryPostProcessor 对应的Bean 已经注册了， 但是@Value @Autowire 这些注解并没有注入， 都是Null 的
> ! 但是注意， 通过Xml ref 申明的依赖， spring boot, Bean直接调用的， 会注入Instance, 容易理解 Instance 内也是 空的。(所以会有循环依赖的情况)
todo： bean internalBean 的 配置加载方式