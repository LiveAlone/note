# spring cloud common

基础的概念规范的定义

- org.springframework.cloud.client.ServiceInstance
  - 定义具体Service的 Host, port 访问, https secure 方式, Uri, Metadata 的服务信息
- org.springframework.cloud.client.serviceregistry.ServiceRegistry
  - 服务注册， 解除注册方式
  - Registration 获取ServiceID
- LB 通过装换HttpRequest 方式， 不同Service 的请求
- DiscoveryClient 获取远程服务的 SERVICE_INSTANCE