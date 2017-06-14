# spring resource load (配置文件加载)

## spring Resource 相关配置定义

- Resource InputStreamSource 资源接口定义
  - ContextResource 不同环境ResourcePath 的路径, ClassPathContext, ServletContext, FileSystemContex 配置环境
    - ClassRelativeContextResource class 相关的路径Resource
    - ClassPathContextResource, ServletContextResource, FileSystemResource 不同环境Resource 的配置信息。
  - EncodedResource client 使用Resource　解码配置方式
    - GzippedResource 资源配置解压
- VersionedResource 资源版本的 Resource 配置方式。
- WritableResource resource 支持写的资源配资方式。
- AbstractResource 资源的抽象管理方式
  - PathResouce Files 文件阅读方式, 获取Path 文件的路径。
  - DescriptiveResource 资源部的描述配置信息。
  - BeanDefinitionResource beanDefintion spring bean 定义Holder
  - GzippedResource 包含Resource original, gzipped 的 原有，压缩后的 资源配置信息。
  - FileNameVersionedResource 包含 Resource, Version 的版本配置信息。
  - InputStreamResource 流 输入Resource 配置信息
  - VfsResource jboss vfs 配置信息
  - ByteArrayResource -> TransformedResource Resource 对应的 byte[]  数据转换方式。
  - FileSystemResource -> FileSystemContextResource 文件环境的配置方式。
  - AbstractFileResolvingResource 抽象 FileResolver, 路径Context Resolver 配置定义
    - UrlResource
    - ClassPathResource
    - ServletContextResource

## ResourceResolver 资源解析方式

- ResourceResolver wbe request 请求， 获取Resource 配置信息。
  - AbstractResourceResolver 轴向Resource resolver
    - 实现 CachingResourceResolver，VersionResourceResolver，GzipResourceResolver，PathResourceResolver，WebJarsResourceResolver
- DefaultResourceResolverChain resouce resolver 链支持配置。

## Load Class 接口定义

- org.springframework.core.io.ResourceLoader 加载 ClassPath file system resouce 配置信息。
- ResourcePatternResolver ResourceLoader的扩展信息， classpath* 配置文件加载
- DefaultResourceLoader 默认ResourceLoader 实现方式，ResourceEdit 使用方式。
  - UrlResource 对应的 URL 资源信息 或者， URI 资源定位。
  - 资源加载过程：1 ProtocolResolver 加载对应的Location 获取资源信息 2 ClassPathResource 获取本地资源（'/' 或 classpath 路径，） 3 默认Url 资源
- 实现Class：ClassRelativeResourceLoader　FileSystemResourceLoader　ServletContextResourceLoader，　不同环境Resource 配置方式。

## 通过Factory Processor 去获取执行Properties 配置信息 [链接](http://blog.csdn.net/dalinsi/article/details/53037957)

- BeanFactoryPostProcessor bean 先执行的配置。
- PropertyResourceConfigurer 
- PropertiesLoaderSupport mergeProperties 合并配置文件
- PropertiesLoaderUtils ＃ fillProperties 通过IO读取资源文件。
- AbstractBeanFactory # resolveEmbeddedValue 解析占位符
- DefaultListableBeanFactory #　resolveDependency　方法
- AutowiredAnnotationBeanPostProcessor　属性自动注入

## spring event 相关时间配置信息

- ApplicationEvent 事件Parent, 不同的事件实现方式。
- ApplicationListener 时间监听器， 监听ApplicationEvent 事件发送
- ApplicationEventMulticaster 批量的事件发送机制
  - AbstractApplicationEventMulticaster，SimpleApplicationEventMulticaster 事件发送的抽象实现方式。
  - ListenerRetriever 监听Listener ListenerBean 的配置方式。

## spring boot event 事件监听相关

- SpringApplicationRunListeners spring boot 对应SpringAppliction 不同的执行状态的时间监听发送器。
- SpringApplicationRunListener boot listener 实现接口， 监听不同状态事件信息
  - EventPublishingRunListener spring App 运行监听事件发送器, 发送不同监听状态事件
  - !! spring 通过事件发送，完成Processor 加载方式

## spring boot envoirment 的启动配置方式(prepare envoirment 环境配置加载方式)

- prepareEnvoirment 环境的基础配置上使用。spring run 启动环境配置。
- 环境创建对应的Envoirment, web 环境 StandardServletEnvironment。 添加默认 PropertySource 配置
- 配置启动参数 args 到Env 环境中
  - 添加 MapPropertySource 配置环境中 commandLineArgs, 添加对应的配置到
  - 添加 Profile 环境配置， 激活Profile 配置设置
- SpringApplicationRunListeners spring boot 事件监听方式。
- springBootListener 构建过程监听， 发送 ApplicationEnvironmentPreparedEven(application, args, envoirment) 配置
  - 通过 SpringApplication 获取Spring.factory 对应的Listener 的配置信息。
- 其中Listener 中有 ConfigFileApplicationListener 的监听器加载配置文件信息
  - ApplicationEnvironmentPreparedEvent 监听到对应的Event 的事件类型， 加载配置文件
  - 配置文件的加载方式：
    - EnvironmentPostProcessor spring.factory 获取所有的 Processor
    - 当前ConfigFileApplicationListener 也是一个Env Processor 的配置方式
    - Order 排序， 执行对应的 Processor
    - postProcessEnv 执行获取配置文件信息
- org.springframework.boot.context.config.ConfigFileApplicationListener postProcessEnvironment 配置文件加载
  - Application Resource Loader 获取资源， 添加资源 MapResourceLoader 配置方式。
    - 加载不同的 Profile 的环境先， 添加对应的 ResourceMapper 的配置方式。
    - configureIgnoreBeanInfo 配置系统 忽略的Bean 等
    - env 现在就加载完成了， 可以绑定对应的 SpringApplication 完成继续执行。
- Listener 执行完成，Processor 可能修改WebEnvoirment, env 对象转换。
- 返回Envoirment 环境对象配置

##　SpringApplication Listeners 加载时间

- spring application initlization 时间, 加载 spring.factory 中的Listener, 监听Spring 的事件发送机制。
- 通过ConifgFileListener  监听EnvPrepareEvent , 通过Processor去加载 Load 不同的文件配置。

> spring 加载完成了 Envoirment 环境， 创建Application ， 通过ApplicationContext 绑定当前的环境配置信息。