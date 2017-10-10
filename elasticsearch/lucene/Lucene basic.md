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
  - ```Store.NO``` 
  
  42 Page