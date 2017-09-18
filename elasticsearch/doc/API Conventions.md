# API Conventions

Es 支持

## 多个索引支持

- 搜索支持多个Index
  - ```test1,test2,test3```, ```test* or *test or te*t or *test*```
  - Add + Remove - 方式, 加减Index
- 多索引查询, 支持Url参数
  - ```ignore_unavailable``` 设置 true false, 是否忽略Index , 不存在 或者 close 的Index
  - ```allow_no_indices```, 通配符Index 是否匹配成功， false, 没有匹配到Index， 有异常信息
  - ```expand_wildcards``` 指定匹配的Index 处于 open close 的状态。 指定 ```open,close```
  - 默认 ```open,close``` 指定状态

## index name 支持时间函数

时间函数, 通过时间范围，搜索对应的Index

- 格式 ```<static_name{date_math_expr{date_format|time_zone}}>```
- demo ```GET /<logstash-{now/d}>/_search``` 通配符 当前时间 logstash-2024.03.22

## 通用配置

- ```pretty=true``` json 格式化, 仅仅debug 的条件。 ```?format=yaml``` yaml 格式 format
- Date Math 支持表达式的方式
  - +1h - add one hour
  - -1d - subtract one day
  - /d - round down to the nearest day
- Response Filter 返回过滤
  - ```filter_path``` 返回过滤路径, 支持 * 正则匹配的方式
  - 通过 char ```-``` 去除对应的字段
- 不同的单位支持
  - time unit
  - byte size unit
  - distance 距离单位
- 模糊的配置
  - 类似 范围, ```0...5, >5 等范围表达```
- 错误跟踪 ```error_trace=true``` 返回错误的具体信息
- ```rest.action.multi.allow_explicit_index: false``` 拒绝URL 指定Index 名称， 防止覆盖
