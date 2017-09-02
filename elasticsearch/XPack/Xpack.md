# X-Pack

用于机器学习, 图, 监控等。
elasticsearch.yml, kibana.yml, logstash.yml 配置

## elasticsearch config

- meachine learning ```xpack.ml.enabled``` job datafeed 的配置
  - ```node.ml``` 节点 是否Run Jobs
- Monitoring Setting ```xpack.monitoring.enabled``` 设置 es.yml, logstash.yml, kibana.yml 配置
- ```xpack.monitoring.collection``` 设置Es 集群节点
  - ```xpack.monitoring.collection.cluster.state.timeout``` 收集集群节点状态 默认 10m
  - ```xpack.monitoring.collection.cluster.stats.timeout``` 集群数据收集 10m
  - ... 等等配置
