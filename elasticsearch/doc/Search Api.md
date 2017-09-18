# Search API

- routing 通过routing 定位搜索 Shard
- Search Cancel 较长时间查询, 去除搜索

## Search

- 多索引，多Type 类型搜索查询 ```GET /kimchy,elasticsearch/tweet/_search?q=tag:wow```, ```GET /_all/tweet/_search?q=tag:wow```
  - 设置 Shard 尽量数据均衡， 避免数量较大的 Shard
  - ```action.search.shard_count.limit``` 拒绝 request 命中 过多的shard
  - request param ```max_concurrent_shard_requests``` 设置最大的命中 Shard

## Url Search 搜索

- param
  - q, 查询字段
  - df 默认返回字段
  - lenient 格式化返回的 Error Respose
  - explain score 的计算方式

## 请求搜索的Body 内容

- search 返回的消息结构
  - timeout 是否超时
  - from, size 分页
  - search_type 搜索方式
  - request_cache shard 缓存搜索
  - terminate_after=5 ```每个分片, 满足 5 条， 在response 返回 terminated_early=true```
  - batched_reduce_size 避免某个 Request 使用过多的 Memory

### Query

### From Size

结果分页， From+Size = index.max_result_window
通过 Scroll , Search After 提高查询的效率

### Sort 排序

- 可以指定 Field, _score, _doc 通过Index score 的排序
- Sort Order, ```desc, asc``` 不同排序方式
- sort mode option, 对应 Field 有多个数值的时候, 通过聚合函数指定对应的数值
  - ```min max sum avg``` 等
- nested object 内部对象的排序
  - ```nested_path``` 内部对象数值获取的排序 direct_field
  - ```nested_filter``` 内部对象多值的形式, 过滤内部对象
- Missing Value, 设置默认 sort 数值
- 通过距离排序, 指定sort
- 通过 Script 脚本执行排序
- ```track_scores```, 默认排序方式，默认不用 match 不会计算Score,  ```track_scores=true``` 计算分数
- 内存考虑
  - 排序的字段会 Load 到内存中, 内存中足够的字段存 排序字段
  - String 类型 不应该 analyzed / tokenized
  - 数值类型, 建议设置 Short, Integer, Float 类型

### Source Filter

- ```_source``` 指定对象 false, obj*
- ```include,exclude``` 包含排除Field

### Fields

- ```stored_fields``` 指定Mapping 确定 stored=true
- ```"stored_fields" : []``` 返回 _id, _type 类型

### script_fields

- 通过脚本，生成 field, doc 返回
- script 获取 doc 字段， ```doc['field'].value```,```param['_source']```
  - ```doc['field']```, 会获取缓存的字段,  _source 会直接获取Field (!! 很慢 !!)

### Doc Value Fields

- field 不会 Stored 存储倒排索引
- request Field 会Cache 缓存, 有Memory 消耗

