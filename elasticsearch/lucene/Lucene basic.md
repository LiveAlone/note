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






