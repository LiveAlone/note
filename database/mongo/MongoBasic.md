# mongo basic 文档类型数据存储, 简易开发, 度量

- 高性能, 高可用, 自动伸缩
- 基于文档方式, Json Docuemnt 不同语言支持, 内嵌 Array, Document reduce join, 灵活的模式
- 特点
  - 高性能
    - 内嵌对象减少 系统磁盘IO
    - 内 doc, array 建立index, 快速查询
  - 查询支持, 数据聚合, text, Geo类型支持
  - 高可用 备份设施
    - 自动 failover, 故障抛弃
    - 提供 冗余redundancy, 增长数据
  - 水平扩展方式
    - 集群Sharding 支持
    - mongo3.4 支持Zones, 不同分片
  - 支持不同的存储引擎
    - WiredTiger Storage Engine
    - MMAPv1 Storage Engine
- Alts 云Cloud 的支持，
  - compass GUI 支持
