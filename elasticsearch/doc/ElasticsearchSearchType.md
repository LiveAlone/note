# SearchType 详解

SearchType 的搜索类型, QUERY_THEN_FETCH,QUERY_AND_FEATCH,DFS_QUERY_THEN_FEATCH和DFS_QUERY_AND_FEATCH

## 搜索过程

- Search query 不同的shard 请求数据， gather聚合数据, 排序返回给用户
- 搜索问题
  - 数量 count = 10, 每个shard 一共 count * shard count = ? 数量比较多
  - 计算的分片数值，通过各自的Shard 计算的, 计算的分数可能准确。

## query and fetch

每个 shard 查询， 返回 shard * n 的数值， 各自分片上计算对应的数值， 统一进行排序。

## query then fetch

查询 返回 Score 的信息， 统一收集排序， 再去 每个Shard 获取Doc 内容。 size * n

## DFS query and fetch, DFS query then fetch

精度较高, 初始化发散， 获取每个分片的 词频率， 文本的频率
搜索的效率比较低, 可能多次请求分片