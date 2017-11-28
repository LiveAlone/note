# Kibana

Kiana 列数据 图标展示. 图标的动态创建展示。 实时的数据查询
Kiana 与 elasticsearch 版本最好同步(并不向上兼容)

## configure

- server.port 默认端口 5601
- server.host 默认 localhost
- server.basePath 默认 Kibana 路径
- server.maxPayloadBytes request 最大请求大小
- server.name hostname
- server.defaultRoute 默认```app/kibana```
- server.customResponseHeaders 默认 response Head
- elasticsearch.url es url 路径
- elasticsearch.preserveHost 默认 True
- Kibana.index 默认 index名称 .kibana
- ```elasticsearch.username: and elasticsearch.password``` es 集群名称
- ssl 相关
  - ```server.ssl.enabled``` 默认False
- 设置 Elasticsearch 相关的请求 超时时间

## Access Kibana

localhost:5601/status 查看Kibana 当前的 状态

## Connect Es with Kibana

- Index Patten 过滤 Es中的名称

## 生产环境Es 配置

- 通过 X-Pack 配置用户权限
- Enbaling SSL
  - kibana.yml ```server.ssl.enabled, server.ssl.certificate and server.ssl.key```
