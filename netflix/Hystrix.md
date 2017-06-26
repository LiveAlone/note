# Hystrix doc 分布式 延时，容错工具
Hystrix 提供 对于三方系统， Service, 3 party 库，的延时，容错特性。
防止 服务的雪崩，提供弹性容错。 在复杂的分布式系统中。 当失败不可避免的情况下。

## 基础实现功能

- 延时 容错
  - 防止雪崩，倒退， 优雅的服务降级操作。 快速失败，和 迅速的恢复
  - Thread and semaphore isolation with circuit breakers.
- 实时操作
  - 实时的监控， 配置文件的修改
  - Service Property 改变，快速的扩散配置
- 并发性
  - 并行执行，同步请求Cache 配置

## dashboard 看板的方式 监控Hystrix 的执行方式
看板 监控服务的状态

## Hystrix 的方式

- 分布式环境，一些服务Fail了，Hystrix 通过 延时等待 和 错误容忍逻辑。 
- Hystrix 隔离 Service 之间的调用方式， 通过这 防止 雪崩， fallback 选择， 提高系统的 resiliency (回弹 恢复)

## hystrix for 方式

- 对于 三方库，服务的调用， 超时 失败（尤其网路原因） 给予保护
- 防止 雪崩 在复杂的分布式系统中
- 快速失败恢复操作
- 回退， 平滑降级
- 近乎 实时的监控， 预警， 操作控制

## hystrix 的处理方式

- 复杂分布式系统 一个Service 失败， 可能会引起雪崩， 整个服务的瘫痪

## hystrix 的设计基础

- work 方式：
  - 防止对容器的 Single 依赖方式
  - 动态Load config, 快速失败， 不会队列阻塞
  - 提供回退，降级， 用户从失败恢复
  - 通过 ```bulkhead, swimlane, and circuit breaker patterns``` 方式， 限制对 单个的依赖
  - 提供 实时的监控， 度量， 预警， 去 实时的 服务失败发现
  - 设置 recovery-time， 低延时的 实时 Property 的动态监听。  允许 实时的 操作修改，快速反馈
  - 失败保护， 不仅仅网络，Client 失败等

## hystrix 对应的实现方式

- Wrapper 所有的 外部请求Calls
- 请求超时定义的 时长， 
- 每个依赖维护一个 线程池， 数量Full， 会立即失败。
- 衡量 成功 失败（exception）， 超时， 线程拒绝
- 如果 错误的达到一定的比例， 快速的 路由断开。
- 提供 回退逻辑，请求失败，拒绝的条件。
- 实时的监听 Config Metric

![hystrix design](https://github.com/Netflix/Hystrix/wiki/images/soa-4-isolation-640.png)

## hystrix 的工作 方式

- 实现 流程图 ![flow chart](https://raw.githubusercontent.com/wiki/Netflix/Hystrix/images/hystrix-command-flow-chart.png)
- Construct a HystrixCommand or HystrixObservableCommand Object
  - Command 构建Request的方式， 依赖方式。
- Execute the Command 
  - execute 执行 blocking 方式
  - queue 队列方式， 监听 single response
  - observe 订阅 Observer 复制 souce Observer
  - toObserver 执行 Hystrix, 提交Response
- response cache 缓存方式
- Circuit Open 检验 Pool 是否 可用
- Is the Thread Pool/Queue/Semaphore Full
- Calculate Circuit Health
- Get the Fallback
- Return the Successful Response

## circuit breaker 循环校验方式，validate success fail, 
![circuit breaker](https://raw.githubusercontent.com/wiki/Netflix/Hystrix/images/circuit-breaker-1280.png)

## 隔离方式
- hystrix 隔离并行的查询方式 ![isolate](https://github.com/Netflix/Hystrix/wiki/images/soa-5-isolation-focused-640.png)
- 不同 依Decy 默认线程池， 线程执行 查询依赖方式 ![thread pool](https://raw.githubusercontent.com/wiki/Netflix/Hystrix/images/request-example-with-latency-1280.png)
- 通过线程池的方式， 保护线程失败方式。
- 实现隔离的方式
  - 后台依赖 dozen 服务
  - 每个服务有各自的 依赖 Libaray
  - Client 服务Service 一致变化
  - 客户端 服务 添加新的 Network 请求。
  - client 库， 重试，数据Parse, 缓存，
  - client 库 黑盒配置。对于 配置，实现， 不对使用者透明传输
  
![thread pool ](https://raw.githubusercontent.com/wiki/Netflix/Hystrix/images/isolation-options-1280.png)