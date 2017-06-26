# servo app metricx 信息的 push, expose
- Leverage JVM : java 监控接口， 不同Tools 
- Simple: 监控推送 简单实现，
- Flexible Publish : 不同系统的消息推送， systems, log等。

## 不同的工具配置方式
- JMX
- Annotation
  - MonitorTags 编译时候，
  - Monitor 监控使用次数， 

## datasource Types
- gauge number 数量统计， demo: 数据库连接的数量
- counter: 可增加的 数量， 统计 连接次数，调用次数。
- informational , 监控系统 信息。

## Monitor Registry 
> 需要监控的对象， 通过继承实现接口， 实现接口的注入
1. DefaultMonitorRegistry
2. JmxMonitorRegistry

## Publishing
metrics 注册以后， 可以polled 或 push 对应的信息
- MetricObserver 接受Metric 内存中， export file, CloudWatch 监控方式
- MetricPoller 接受 Metric source 不同的Resource 
- MetricFilter 使用在 Poller Source 的方式

## Observers
- MemoryMetricObserver 内存中信息修改
- FileMetricObserver 硬盘文件信息修改
- AsyncMetricObserver

## Pollers
- MonitorRegistryMetricPoller monitor register 注册监听
- JmxMetricPoller jmx 消息监听
- 