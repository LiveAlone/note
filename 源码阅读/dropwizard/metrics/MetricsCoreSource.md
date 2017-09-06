# Metric Core Source

## Report metric 记录的报告信息

- ScheduledReporter 定时Report
  - CvsReport, Slf4jReport, ConsoleReport 不同的输出方式
- JmcReport 通过MBeans 的方式 获取Metrics 的信息

## Metric不同计算统计方式

- Counter 统计次数
- Meter 类似UNIX 1min, 5min, 15min 对应CPU 的均衡
  - 多核CPU, LA = n 对应的核数