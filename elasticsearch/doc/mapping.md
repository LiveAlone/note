# Mapping

- document 的格式定义, Fields, stored, indexed
  - string 类型, full 全文检索方式
  - filed 类型 Number, Dates, 定位等
  - 设置 field, index stored, 状态
  - filed format 格式方式， mapping 动态变化方式
- Mapping Type
  - Meta-filed 对应 _index, _type, _id, _source doc信息
  - Fields 对应 properties
- Field dataTypes
  - 基础类型 text,keyword,date,long,double,boolean,ip
  - object, nested 的 对象
  - 特殊类型 ```geo_point, geo_shape, completion```
- 设置阻止 mapping explosion
  - 太多的Fields 可能造成 内存溢出，不同情形恢复
  - ```index.mapping.total_fields.limit``` 设置 mapping filed 数量, 默认 1000
  - ```index.mapping.depth.limit``` index object 的深度， default 20
  - ```index.mapping.nested_fields.limit``` nested object 数量， default 50
- 动态 mapping
  - 建议使用 明确的mapping格式
- 更新 exiting mapping
  - 存在 type field, mapping 不能更改
  - 不同的 type 一个Field, 字段共享的

## Removal mapping types

- Es 以后版本 逐渐移除 多Type 的支持

## Field datatype 

- Core 数据类型
  - text
    - 文本类型，设置全文检索， 设置 Analyzer, 
    - warning: 字段 不用排序, 不用聚合方式（建议使用 keyword）
    - 使用 keyword，同事 text, 设置 multi-field 方式
    - text 参数设置
      - analyzer ， analyzed, search_analyzer
      - boost 默认 1.0
      - ```eager_global_ordinals``` load early 当 refresh 时候, 默认 falses
      - fielddata 默认false, 字段是否 sorting, aggs, script
      - ```fielddata_frequency_filter``` values 是不是 load 如 缓存中
      - fields 同一个字段，设置不同格式
      - ```include_in_all``` _all 是否使用
      - index, index_options 默认 true 是否search 搜索到
      - norms
  - keywords
    - 字段存储， sort，aggs 处理方式
  - Number 数据类型
    - long integer, short, byte, double, float, half_float, scaled_float
  - Date type
    - format 设置存储格式
  - Boolean Type
    - 默认 False: false off, no, 0 等
  - Binary Type 二进制类型
  - range datatype, 设置范围类型
- 复杂类型
  - Array Type 没有 专用, value 可设置Array 类型
  - Object, Nested 类型
- 特殊的数据类型
  - Completion dataType 自动补全的 suggester 方式
  - token_count string 设置 Integer 类型
- Join 定义 树形 Parernt_Child 的格式类型
  - PUT 定义 内容， 指定 Parent id 