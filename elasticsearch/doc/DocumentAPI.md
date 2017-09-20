# Document API

document CRUD API 接口配置

## Index API

- PUT 请求返回参数
  - ```_shard```, 返回 success, fail 的 分片
- Index 自动创建方式, ```action.auto_create_index=false```, put 请求
  - ```index.mapper.dynamic=false```, 对单个 Index 设置自动配置
- Version PUT 请求通过乐观锁的方式, 校验 Doc 版本校验
- Operation Type
  - ```op_type=create```, ```_create``` 请求操作 创建方式
- Routing shard 的路由方式

## CRUD

- GET,DELETE Http delete 请求方式
- ```_delete_by_query``` 通过 Query 删除执行方式
- update, upsert, 插入, 存在插入删除方式
- Update By Query API
  - 之心Query, 查询对应的 Document
  - script 脚本 执行方法
- multi get 多个 Document 获取方式
- bulk api 批量 Index Update 执行方式
- ReIndex 索引 迁移 镜像复制