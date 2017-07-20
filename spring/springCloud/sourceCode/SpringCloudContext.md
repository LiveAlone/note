# cloud context config

> 提供 bootstrap, encryption, env endpoints 的监控配置信息

- spring context 定义对应启动的Configuration 的配置信息

## application context 启动配置方式

- 通过spring.factory 添加ApplicationListeners 应用启动监听器
- BootstrapApplicationListener 监听器执行, 修改Application， evn 的环境的配置。 （自己的Listener 会返回）
  - ```spring.cloud.bootstrap.enabled``` 判断bootstrap 的执行,（这个配置 在application.yml 不可以，这个时候还没有加载到这个内容）
  - ParentContextApplicationContextInitializer Parent Initlizer 初始化可以
  - 创建Bootstrap Context, 通过SpringApplicationBuilder 创建一个
- ParentApplication 加载BoostApplication config
  - 通用的 Env bootstrap 先加载， bootstrap 后加载的方式

## 远程的配置加载

- 执行时间 Applicaiton PrepareContext 的时候获取远程的 配置 （！！ 不是 Bootstrap Application 的环境）
- prepareContext 添加Initlizer
  - AncestorInitilizer 设置 ParentApplicaiotnContext, 关联boostApplicationoContext, currentContext
  - DelegatingEnvironmentDecryptApplicationInitializer decrypt 解密Property 的配置文件
  - PropertySourceBootstrapConfiguration 继承 ApplicationContextInitializer 启动配置
    - 调用Server 服务, 获取 Config配置信息， 添加 github 的配置文件
    - 添加 composite 的配置信息， Env 完成配置。
> 通过Listener 的方式, 在PrepareEnv 执行获取环境配置。 PrepareContext, 执行Initlizer, 覆盖环境变量。