# spring cloud zuul
> 通过 zuul 路由不同的Service 请求方式。

## zuul 提供的功能

- 基础功能
  1. 权限认证
  1. Insights
  1. 压力测试
  1. 动态路由
  1. 服务迁移
  1. 负载均衡
  1. 安全， 静态响应处理，主动流量管理
- 配置提供
  - ```zuul.host.maxTotalConnections```, ```zuul.host.maxPerRouteConnections``` 最大每个路由的链接数量
- 通过 SpringCloud 反向代理的方式
  - 代理 UI 访问一个或者 多个 后台服务
  - 避免每个服务的 CORS, 权限验证
- zuul 提供 Filter的 过滤方式，通过过滤器的方式， 权限校验等。
- zuul 提供 tcp 链接方式， 线程池 Plugin 的插件方式
