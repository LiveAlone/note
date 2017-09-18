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

### Post Filter

- 通过请求过滤，aggs 聚合字段过滤方式

### highlighting 高亮显示

- 高亮匹配的字段的时候， ```store=true``` ， 否则加载 _source 字段
  - 字段支持 通配符, 匹配显示高亮
- Plain highlighter 
  - 默认 ```Plain``` 高亮, 使用Luence 高亮
  - 多字段查询时候, 可能Slower
- Postings highlighting
  - ```index_options``` 设置成 ```offsets``` s
  ```
  PUT /example
  {
    "mappings": {
      "doc" : {
        "properties": {
          "comment" : {
            "type": "text",
            "index_options" : "offsets"
          }
        }
      }
    }
  }
  ```
  - posting highlighting
    - faster, 文档 large, 性能较好
    - less disk, highlights term
- Fast Vector highlighter
  - ```term_vector``` 设置 ```with_positions_offsets```， 使用条件：
    - field 比较大的时候， >1MB
    - 定制 ```with_positions_offsets```
- 高亮片段
  - ```{"fragment_size" : 150, "number_of_fragments" : 3}``` 
- order 高亮匹配返回多个字段, 设置排序方式
- highlight query, 高亮显示， 设置 QueryDSL, 返回高亮字段的过滤方式

### Rescoring

- rescore 每个Shard 上， 返回之前， 执行的 Query
- sort 执行时候， 不会 rescore
- 返回 Top-K 时候，设置 window-size 
  - ```query_weight``` and ```rescore_query_weight``` 默认1

### Scroll 滚动查询

- scroll, 对于 Result 的结果集Large 时候， Page 分页
- scroll 时候, 镜像缓存， 修改不会影响 分页结果
- ```index.max_slices_per_scroll``` slices 分页 结果集

### perference

- ```_primary``` 执行Primary Shard
- ```_primary_first``` 执行 primary shard， 失效备用时候
- ```_primary_first```, ```_replica_first``` 
- ```_local``` 默认Local 分片
- ```_prefer_nodes:abc,xyz``` 默认shard 节点
- String 选择节点配置

### explain 设置TRUE， explain 信息

### Version : true 返回版本

### index boost

### min_score 最少Score 分数

### 按照特定的字段， 折叠去重

- collapse 查询后， 通过特定的字段聚合

## search template

- 通过模板的方式， 参数 设置 查询Jsons

## multiply request 设置 template 模板格式设置

## search shard API 搜索特定 分片

## suggester 关键词搜索， 自动补全的功能