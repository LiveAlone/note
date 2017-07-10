# spring cloud zuul config
zuul 的集群方式， 实现智能路由的方式。
负载均衡 zuul, nginx的方式。  服务网关 zuul 集群， 最后到 对应的 Service 服务。
通过 URL 映射 不同的 Service 服务方式。

## 实战
1. 通过 ```/p1/**``` ```/p2/** ``` 访问不同的 Service
1. zuulFilter 的过滤方式， 权限， pre, routing, post, error, filterOrder 等方式。