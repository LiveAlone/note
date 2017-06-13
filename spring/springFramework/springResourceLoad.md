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

## spring boot envoirment 的启动配置方式
- prepareEnvoirment 环境的基础配置上使用。
- 通过Event 方式， 通过Listener 方式， 添加配置文件的修改方式。
```
ConfigFileApplicationListener
AnsiOutputApplicationListener
LoggingApplicationListener
ClasspathLoggingApplicationListener
BackgroundPreinitializer
DelegatingApplicationListener
FileEncodingApplicationListener
```
- ConfigFileApplicationListener 加载对应的 spring.application 文件配置。 发布对应的事件 ：ApplicationEnvironmentPreparedEvent
- PostProcessors 处理方式 执行环境配置。 ConfigFileApplicationListener 对应的 也是一个 Processor
- listener addConfigurationProperties 函数添加 配置信息。 
