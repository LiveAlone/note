# spring cloud common

基础的概念规范的定义

- org.springframework.cloud.client.ServiceInstance
  - 定义具体Service的 Host, port 访问
- org.springframework.cloud.client.serviceregistry.ServiceRegistry
  - 服务的注册, 解开注册的配置方式
- LB 通过装换HttpRequest 方式， 不同Service 的请求
- DiscoveryClient 获取远程服务的 SERVICE_INSTANCE