# Metric Core Source

## Report metric 记录的报告信息

- ScheduledReporter 定时Report
  - CvsReport, Slf4jReport, ConsoleReport 不同的输出方式
- JmcReport 通过MBeans 的方式 获取Metrics 的信息

## metric 不同的统计方式
- Metric 基础的统计方式
- Histogram 统计设置的变化
  - Sampling 返回当前 Snapshot 返回当前镜像
  - Reservoir 数据流， 统计数据的Value
  - Counting 获取Count 数值
- Counter 数量统计
- Gauge 定制化的数据统计 Value
  - JmxAttributeGauge 获取 JXM 的Bean
  - Ratio 比率 分子 分母的 
  - CacheGuage 
  - DerivativeGauge guage 的驱动配置方式
- Metered 不同时间 发生概率 (5 min 10 min)
  - Meter
  - Timer
- MetricSet 集合方式 不同的Metric

