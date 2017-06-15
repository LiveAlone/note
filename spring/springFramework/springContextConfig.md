# spring context config 加载配置方式

## Application 创建加载方式

1. SpringApplication run 创建对应 ApplicationContext 环境配置。
1. createApplicationContext 创建对应的ApplicationContext
  - SpringApplication 指定对应 applicationContextClass
  - 默认 不指定 web 环境， AnnotationConfigEmbeddedWebApplicationContext， 默认环境： AnnotationConfigApplicationContext
  - 默认创建DefaultListableFactory 默认工厂创建方式。
1. FailureAnalyzers 创建方式。当前ClassLoader 方式，spring.factory 加载对应BeanDefinition错误配置
  - spring.factory 创建对应的 Analyzer 配置方式。
    - org.springframework.boot.diagnostics.analyzer.BeanCurrentlyInCreationFailureAnalyzer
    - org.springframework.boot.diagnostics.analyzer.BeanNotOfRequiredTypeFailureAnalyzer
    - org.springframework.boot.diagnostics.analyzer.BindFailureAnalyzer
    - org.springframework.boot.diagnostics.analyzer.ConnectorStartFailureAnalyzer
    - org.springframework.boot.diagnostics.analyzer.NoUniqueBeanDefinitionFailureAnalyzer
    - org.springframework.boot.diagnostics.analyzer.PortInUseFailureAnalyzer
    - org.springframework.boot.diagnostics.analyzer.ValidationExceptionFailureAnalyzer
    - org.springframework.boot.autoconfigure.diagnostics.analyzer.NoSuchBeanDefinitionFailureAnalyzer
    - org.springframework.boot.autoconfigure.jdbc.DataSourceBeanCreationFailureAnalyzer
    - org.springframework.boot.autoconfigure.jdbc.HikariDriverConfigurationFailureAnalyzer
  - 对应 BeanFactoryAware 注入对应的 BeanFactory， 解析时候， 错误Analyse 配置方式
1. prepareContexnt context 默认前 准备。
  - config 解析完成的 Envoirment 配置环境。
  - postProcessApplicationContexnt 对应BeanFactory 注入 internalConfigurationBeanNameGenerator， resourceLoader 
    > 默认用户自定义Bean
  - listeners 监听对应的Context， publish ContextPrepared 的事件。
  - beanFactory 注册 springApplicationArguments 参数Bean配置信息。
  - 注册 springBootBanner 对应的 printBanner 配置方式。
1. refresh Contexnt !! 刷新Bean的配置信息。
1. afterRefresh 执行 ApplicationRunner， CommandLineRunner 用户自定义的 Runner 的配置方式。

## Context Refresh 执行过程
- Spring 事件发送配置， 通过BeanFactory在 bean.factory 对应的Listener Bean 的配置。
- 与Multicast 对应的Listener 不相同。
- 刷新完成，启动对应的容器配置。