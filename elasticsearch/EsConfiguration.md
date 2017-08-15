# Es Configuration

## index name 匹配方式

- 正则 Filter 过滤匹配方式
- 日期格式 name-date 搜索索引

## Document API 方式

- GET API
  - get 获取实时的数据, 数据Post, not refresh, 引发Refresh
    - 设置 realtime=false 不会引发 Refresh
  - ```_type``` optional type 类型
  - ```_source, _source_include _source_exclude``` 指定Include Exclude 的字段类型
  - ```stored_fields``` 获取StoreField 如果没有Store 不会返回对应的结果
  - Routing 指定对应的路由名称
  - Preference 当 replicas 获取Request 请求的时候
    - ```_primary``` 请求主分区 获取请求结果
    - ```_local``` 本地 Replicas 请求数据
  - ```refresh=true``` get 获取数据前 refresh index 数据内容
- DELETE
  - 删除Child 指定删除的 Parent Id
  - timeout
- delete by query api
- update by query
  - 查询方式 改变对应的 Document 的Version 信息， 不回修改 Store 对应的Field 的字段信息
