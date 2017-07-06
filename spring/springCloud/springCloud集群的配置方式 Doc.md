# spring 高可用的配置中心方式

## Spring 高可用的集群架构方式

- spring cloud 单节点的集群 Config 配置方式
  - 启动单点的 Spring Cloud Config Server, 通过该 Server 获取对应的配置信息
  - 配置 github 的路径， 账户配置获取配置信息
  - 配置信息获取  ```http://localhost:8888/{github-filename-path}/foo```, github-file-name 配置方式， applicationName-env.yml
- spring cloud 集群的高可用配置方式
  - 启动 SpringCloudEureka 服务注册的服务
  - 通过 eureka 服务注册的方式， 部署多套 SpringCloud Config 集群中心
