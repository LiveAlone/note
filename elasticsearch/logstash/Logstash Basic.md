# Logstash Basic

实时的收集不同Source 数据，格式化，统一数据格式，用于分析，视图展示。

数据转换流程 Input, Filter, Output plugins, Logstash 对 多Columns 较高的处理能力

集成Elasticsearch, Kibana 日志的实时分析查看。 支持不同的 Input Filter, output 的过滤方式

提供不同的插件支持

- Es 支持的数据
  - Logs And Metrics 支持Apache, Log4j 日志，
    - 获取Syslog, Windows Event Log 日志内容
    - 获取 JMX， Colectd 通过TCP UDP 方式 监听日志内容
  - Web 请求
    - Http Request, 通过拦截请求 监控， Watcher 内容
    - 通过 Http, 拉取 Metric, Performance， Metric 的数据内容
  - 数据存储，Stream  流数据获取 统一
  - 统一的数据监控分析方式


## First Event 事件信息输出

- 数据流： 

  ![数据流](https://www.elastic.co/guide/en/logstash/5.6/static/images/basic_logstash_pipeline.png)

## Parsing Logs with Logstash

- 实际过程中, 一个或多个 Input 数据， Filter 过滤， output 输出对应的 Plugin , 不同的环境
- 通过FileBeat 收集文件日志内容，FileBeat 运行不同 机器上， 收集日志内容进入LogStash
  - Grok filter 过滤日志内容
  - output 内容不同的Server 中














































