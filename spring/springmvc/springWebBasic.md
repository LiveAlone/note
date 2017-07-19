# spring web

## Spring web interface

### servlet 相关

- WebApplicationInitializer ServletContext 初始化相关配置
  - onStartUp 添加 ServletContext 的相关配置
  - servlet 3.0 通过编码的方式, 替换 web.xml 的配置信息。 替换DispatherServlet, configLocation 等配置， Mapping startup, 等配置信息
  - 手动创建DispatcherServlet 的方式，操作简单, 可以手动添加配置， 添加不同的 SpringApplication 的启动方式。
  - spring组件的改动， 支持 init-params, context-params。 DispatcherServlet， FrameworkServlet， ContextLoaderListener, 等
