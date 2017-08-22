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

## search 搜索方式

- search 支持 多 Index, 多 type 类型指定
  - url search 通过Url 查询搜索的方式
- Search Body 消息体结构
  - query 查询 Body 结构体
  - Form Size 分页查询的方式
    - ```index.max_result_window``` 默认 10000 最大条目
    - Scroll SearchAfter 方式 瀑布流的方式
  - Sort 排序方式
    - _score(search 评分), _doc(index order 排序方式)
    - order: desc asc 单个排序字段， 多个 mode： Max Min 等排序方式, 默认 Value 排序方式
    - nest 排序方式： 字段 不同的指定方式
  - source ```_source``` 指定 返回字段类型
  - Script Field 指定 命名 Field 字段
  - Post Filter 对应字段的 过滤方式
    - filter 过滤字段
    - aggs 同样 过滤对应字段，聚合方式
  - high light 字段过滤高亮显示方式
  - SearchType 搜索方式
    - query_then_fetch 宽度搜索方式
    - dfs_query_then_fetch (预查询方式)
  - Scroll 滚动方式, 通过Scroll Id 获取
  - perference primary, replica, local 不同的 查询方式
  - Explain 设置 true , 默认Score 的计算方式
  - Version :true 设置 hit True
  - index boost, 搜索多个 Index, 设置Index 的比例
  - min_score 最小Score 分数
  - Named Query， 指定返回 _name 名称
  - inner hits 设置 Parent/Child or nested 方式， search Document
  - search after, 排序方式， After 对应Id
- Routing 指定， index 指定 Routing, 查询时候 指定Routing
  - hitting 对应 Shard
- global search timeout 搜索时间超时
  - ```search.default_search_timeout``` 默认全局的 Search timeout,  -1 默认没有超市时间
- search cancellation 撤销 search 搜索

## index 的操作方式

- shrink 缩减Index 的方式
- 