# Bean Factory Processor 加载处理方式

- SpringApplcation 创建 ApplicationContext 的时候添加 Processor 
  - 创建 AnnotationBeanDefitnitionReader 定义内部的 Processor
    - 对应 ConfigurationClassPostProcessor
    - AutowiredAnnotationBeanPostProcessor 注册 BeanProcessor
    - RequiredAnnotationBeanPostProcessor
    - CommonAnnotationBeanPostProcessor 
    - EventListenerMethodProcessor
    - DefaultEventListenerFactory
  - 执行 Processor invokeFactoryBeanPost

- initlizer 设置对应 BeanPostProcessor， 在 PrepareContexnt 执行。
  - ConfigurationWarningsApplicationContextInitializer$ConfigurationWarningsPostProcessor
  - SharedMetadataReaderFactoryContextInitializer$CachingMetadataReaderFactoryPostProcessor
  - ConfigFileApplicationListener$PropertySourceOrderingPostProcessor

