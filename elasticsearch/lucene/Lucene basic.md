# Lucene basic

关注文本索引, 搜索功能

- 索引操作（Indexing） 输出 (Index), 大格式Doc 通过索引, 加快搜索的速度
- Lucene 索引快速文本查找方式, 单词查找工具, 索引建立包含模块
  - 文本内容获取，动态活动的, 根据文档内容的修改
  - 静态较小文档加权 （对于很长的字段,  及时匹配到了，不认为关联度会很高）
  - 文档分析, 提取文本内容中的关键词，分析器提供 相近词义分析
  - 文档索引, 加入索引列表
- 搜索组件
  - 建立查询 QueryParse 
  - 搜索查询
    - Bool Pure Query 评分无关， 文档子集的确定
    - 向量空间模型, 计算关联度
    - 概率模型,  全概率方式计算
- 搜索范围 Lucene 没有提供 分布式, 集群的同步方式, Solr, Elasticsearch 提供 外围的服务配置


## Index 核心Class 内容

- IndexWriter 新建打开索引, 索引中添加，删除文档内容。 开辟存储空间通过 Directory 完成
- Directory 路径创建， 添加修改 Index 内容
- Analyzer 通过文本内容分析器
- Document 存储Es 文档中的内容
- Field 字段域， Document 中包含的字段域

## 搜索核心Class

- IndexSearcher 利用Directory 创建的Data 搜索对应的文本
- Term 匹配符合条件内容
- Query TermQuery 不同的查询条件的构建方式

## lucene 基本文档结构

### Document Field

- Document Lucene 基本数据, Field 搜索的Index 建立
- Luence Field 基本操作
  - Document Filed 文本类型， 可以 被Index
  - Field Index 后, 选择存储向量, 检索当前Field 内容
  - Field Value 数值可以单独存储

### 索引过程

- 提取文本, 创建文档
  - 不同文本类型， PDF WPS 类型, 获取文本内容
- 分析文档
  - IndexWrite添加对应的 Document 内容
  - 文本内容分割， Filter 去除无用的Word
- 向索引添加文档内容（分析的结果写入Index 中） Luence 通过倒排索引（inverted index） 方式存储
  - 索引段， Lucene 每个Index, 多个 segments_N 多个段
  - 每个Segment 独立索引，
  - 每个Segment 包含多个文件， 项向量，存储域，倒排索引等
  - Lucene 定时 周期的 合并段操作

### Field 域选择

- 通过 field value 创建倒排索引方式
- 域分析器
  - Index.Analyzed 指定Value 数值分析
  - Index.NOT_ANALYZED 不对String 数值进行 解析, 对 Filed 进行索引。(适用 人名，手机号，学号等 不可风格，精确匹配的String 类型)
  - ```Index.ANALYZED_NO_NORMS```, 不会索引 存储 norms 信息， norms 记录了（索引的时候会消耗内存）
  - ```Index.NOT_ANALYZED_NO_NORMS``` 不索引，不记录norms 信息。 对于 single-token 适用
  - ```Index.NO``` 字段File 不应该被搜索
- field 存储
  - ```Store.YES```, 指定存储Field 原始字符串值 保存 索引中, 通过Index
  - ```Store.NO``` , 指定不存储域值， 通过Index.ANALYZED 方式， 索引大的文本域值

### 域向向量

- 希望检索时候， 文档唯一项能完全， 文档域检索

索引选项|存储选项|项向量|使用规范
---------------------|------|----|------
NOT_ANALYSED_NO_NORMS| YES | NO | 日期姓名等 完全匹配的字符串
ANALYZED | YES | WITH_POSITIONS_OFFESETS | 文档标题，摘要
ANALYZED | NO | WITH_POSITIONS_OFFSETS | 文档的正文
NO | YES | NO | 不会有搜索但是会返回Field 字段的
NOT_ANALYZED | NO | NO | 隐藏关键词

### Field 排序方式

### 多值域

- string,int 基础类型默认支持多值的类型
- 不同类型， 顺序支持加权加载方式

## 文档 域 加权操作

- 加权可以 索引创建， search 搜索时刻加权
  - 搜索加权， CPU 的消耗增加
- 文档加权操作 （文档Doc 添加Boost）
  - setBoost(float) 设置 字段加权
  - boost 默认1, 通过设置Float 添加减少搜索的因子
- 域加权 (doc Filed 字段域加权方式)
  - 加权因子修改 doc 会重新创建

### 加权基准 Norms, 加权因子写入Doc中

- 文档Doc 所有加权合并 Float, 文档有自己的加权（短的Filed 相对权限较高）。 用于搜索计算
- norms 每个Field 分配字节空间， Field 较多， 内存空间占用较多
  - Filed.Index 设置 NO_NORMS 方式
- 索引进行一半 关闭 Norms, 索引重建
  - 有一个文档 有 norms 字段，段合并后， 所有的Segment 都会有Norms 存储空间

## 索引 数字， 日期，时间

- 索引数字
  - whitespaceAnalyzer, StanderAnalyzer 将句子 数字 解析查询方式
  - SimpleAnalyzer, StopAnalyzer 数字会被剔除
  - NumericFiled 数字类型的Field
- 索引日期，时间格式

### 域截取

- IndexWriter 设置 MaxFieldLength，也许 Filed 只有前 固定长度， 会进入搜索的内容

### 近实时的搜索

- IndexReader 会共享 已近打开的Segment 内容
- 索引优化
  - IndexWriter 创建时候 Session 会创建不同的Segment
  - 搜索时候， 每个Segment 搜索内容， 合并每个Segment 的内容。 （Segment 合并的方式， 提高搜索的性能）
  - 索引的优化， 消耗 CPU IO 的资源较高
  - Segment 段合并， 完成之前不会删除, 可能预留3 倍的 优化量的 数据空间

## 并发，线程安全 Lock 方式

- IndexReader 允许多个 reader 同时读写一个索引
- 一个索引， 单个IndexWriter 打开, 会添加 加锁文件
- IndexReader 可以找 IndexWriter 同时执行，IndexReader 记录对应的读取时间
- IndexWriter 线程安全， 可以多线程同时使用

### 索引的Lock 机制

- Index 下 ```write.lock``` IndexWriter 方式打开， 其他创建 产生 ```LockException```

### Index 创建 优化过程

- IndexReader 删除， 定位删除Doc 的id 删除。 IndexWriter 删除对应缓存， IndexReader 打开后， 才会生效。
- 回收删除 磁盘空间
  - 对于倒排索引， 文档的项， 分散不同的位置，
  - 段合并时候， 才会对磁盘的回收
- 缓存刷新
  - 新文档添加，删除文档操作时候， 会先缓存内存中。 不是立即进行， 降低磁盘的IO 操作
  - 刷新操作，Writer 会创建新的 segment, Writer 提交更改，Reader 重新打开后。 会发现提交的更改内容。

### 索引提交过程

- 索引提交过程， commit 两次提交的 之间过程， 不会对Reader 可见的。
- IndexWriter 提交步骤
  - 刷新缓存文档， 提交删除的文档
  - 创建文档同步，挂起所有的 写操作
  - 写入Segment_N 文件， 操作完成， Reader 立刻看到对应的变化
  - IndexDeletionPolicy 删除提交内容
- 两个提交阶段
  - precommit 完成 1,2。 在 3 完成后commit, 出现错误设置 rollback
- 管理多个索引提交方式
  - Luence 只有一个提交，索引聚合多个提交索引的策略方式
- ACID 定义事务，索引方式
  - Atomic writer 提交索引原子性
  - 一致性
  - 原子性， IndexReader 仅仅可以看见 单开之前的 commit 方式
  - Durability 持久性

### 合并段

- 索引 包含段较多， 选择合并单一的段
  - 减少索引段数量（os 对应文件描述 文件句柄数量限制）
  - 减少索引尺寸, 删除操作， Segment 合并后，挂起的删除操作执行， 压缩空间
  - MergeScheduler 定期执行周期， MergePolicy 合并策略
- 段合并策略
  - LogMergePolicy: 测量段字节数,尺寸方式字节数量
  - LogDocMergePolicy: 尺寸当时 文档数量

第三章 配置方式