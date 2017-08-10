# Es How to

## index speed

- bulk request
  - bulk 请求, 相比SingleRequest, 提高Index 的速度
  - bulk 数据大, Es 的内存压力大，及时BigRequest 好的速度， 不超过 couple tens of megabytes, 大约几十 M 就可以
- 使用多个 线程Send Es 请求
  - 多个线程 使用 Es Cluster 集群的资源信息
  - 监控 ```TOO_MANY_REQUESTS (429)``` ```EsRejectedExecutionException``` es 不能接受当前的Index 速率, 指数回退策略
  - 测试方式, 添加线程的数量, 直到CPU/IO 饱和
- Index refresh 默认 1s
  - refresh index 减少 合并的压了
- 关闭Refresh, replicas 启动加载
  - 启动大量的数据加载 ```index.refresh_interval to -1 and set index.number_of_replicas to 0```
    - 可能代码的丢失， 但是 提交加载额速率
  - 可以 Loading Data 后 调整 ```index.refresh_interval and index.number_of_replicas``` 的数值
- 确认 OS not swapping java porcessor
- 给 内存用于文件缓存
  - filesysten cache 用于IO 缓存
  - 确保 half 内存 用于 FileSystem Cache 缓存
- Es Index id 自动生成
  - 使用外部Id， Es 需要校验 Id 是不是重复存在， 不同的Shard 查询
  - auto index, 会跳过这个步骤
- 使用更好的 硬件
  - 如果Indexing 是 IO 绑定的, 更多的 fileSystem Cache, SSD 等快速驱动
  - 使用本地 FileSystem, not remote NFS
  - 不同节点
- index buffer size
  - 设置Es 的 buffer size, 一版 Jvm memory  的 10%

## 通用 recommendations

- 不要返回Large 的文档集合
  - 搜索Search 适合返回Top Match的内容, 不适合返回全量 Match Query
  - 返回全量 使用Scroll API
- 避免 大文档
  - doc ```http.max_context_length = 100M``` 默认最大
  - 可以手动调整， luence 最大 2GB
  - 大文档 影响
    - 内存， 网路，Disk 影响
- 避免 字段重复过多
  - es 依赖 luence, index store data 数据， 更好 敏感Data
    - reason, serach 内部通过 Id 标示Document
  - 方式：
    - 避免在Index 放入不相关的字段
    - 通过 Timestamp， mormalize 字段
    - ```norms and doc_values``` 特定的 fields 指定
      - 'norms' 不需要Score 计算, 仅仅需要 Filter 的字段
      - doc_values, doc 排序聚合不能使用

## search query speed 方式

- 添加FileSystem Cache 缓存
- 硬件存储加速
- document 模型
  - nested 结构， 慢几倍. parent-child realtion 会更慢
  - 避免 aggs, range 等查询
    - Module 添加 type=keyword 的数据类型
    - 避免数值的 聚合方式
- mappings, 不同的库中, Integer Long 类型, 不一定Search 中的类型。
  - 可以 keyword, 类似枚举的聚合类型
- 脚本影响 painless expressions
- query cache 方式, 缓存查询条件, 提升平均查询时间
- 预热 global ordinals

```json
PUT index
{
  "mappings": {
    "type": {
      "properties": {
        "foo": {
          "type": "keyword",
          "eager_global_ordinals": true
        }
      }
    }
  }
}
```

- 预热FileSystem Cache 缓存
  - es 重启, 文件系统的缓存会清空
  - 通过 index.store.preload 方式load 指定的缓存文件

## disk 内存的使用

- mapping 中指定 是否 
  - index（never filter, match）
  - norms:false 不计算分数, 仅仅判断Match的条件
- 不要使用动态 Mapping
- _all disabel
- best_cimpression
- half_double, half_integer 类型使用