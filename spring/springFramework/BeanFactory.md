# Bean Factory 初始化定义方式

## 接口的定义方式
- BeanFactory bean 容器的定义方式， spring 定义不同的Scopre(Singleton, session, etc)
  - load bean definition 不同的source, XML，等配置文件中。 
  - BeanDefinition 执行执行 Order
    1. BeanNameAware 创建Bean Aware Bean Name 的配置方式
    1. BeanClassLoaderAware
    1. BeanFactoryAware
    1. EnvironmentAware 设置Bean 环境配置
    1. EmbeddedValueResolverAware
    1. ResourceLoaderAware
    1. ApplicationEventPublisherAware
    1. MessageSourceAware
    1. ApplicationContextAware
    1. ServletContextAware
    1. InitializingBean
    1. postProcessAfterInitialization
- ListableBeanFactory bean 定义的列表配置，获取列表配置
- AutowireCapableBeanFactory 设置 Autowire 方式
- HierarchicalBeanFactory Parent 继承关系BeanFactory 的定义方式。
  - ConfigurableBeanFactory 定义Bean Scope 作用配置范围
- ConfigurableListableBeanFactory
  > 继承 Autoawire 的方式，忽略依赖

- BeanDefinition bean 的配置定义方式
    - AnnotatedBeanDefinition 获取注解配置的定义
```
  bean factory 解析定义Bean 的配置方式
  BeanDefinition spring bean 解析配置方式
  Metadata 数据信息，MethdoMetadata, AnnotationMetadata, 等不同的配置方式。
```

## spring beanFactory refresh 执行过程 (Spring boot web 环境配置Demo)
- prepareRefresh refresh 初始化配置方式， resource 资源准备
  - initPropertySources 初始化 property source, web 环境， 会初始化ServletContext 对应的Bean Param, ```servletContextInitParams```, ```servletConfigInitParams```
  - 校验Property Resolvable的可配置处理方式。
  - earlyApplicationEvents 初始化 early application events 配置
- obtainFreshBeanFactory 调用子类刷新 内部 BeanFactory
  - refreshBeanFactory 抽象子类刷新BeanFactory 
  - 获取子类的 BeanFactory 配置方式
- prepareBeanFactory 当前ApplicationContext,准备，设置一些已经存在的BeanFactory 的配置Class,  BeanType 的存在类型的注册配置方式。
- postProcessBeanFactory 子类设置 post-process 的方式。
  - 设置BeanFactory post-processing, bean 解析配置完成，后处理（如 Autowire 的配置）
- invokeBeanFactoryPostProcessors 执行BeanFactory 对应的Processors