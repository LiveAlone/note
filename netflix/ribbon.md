# ribbon Client 依赖库

## Feature

- 负载均衡
- 容错
- 不同的通信协议的支持，交互Model
- caching batching

## 提供的Feature 特点配置方式
- 多种 可插拔的 LB
- 与服务发现的 集成
- failure 失败恢复配置方式
- Cloud 云配置方式
- clients 集成 LB
- 使用 Archaius 动态的配置驱动

## 子项目包含的模块配置
- ribbon core: 提供 公共 LB, 公共接口的定义，集成Client LB 和 Client 工厂
- ribbon eureka: 与服务发现的集成
- ribbon httpclient: 通过LB 事项 REST client, JSR-311
