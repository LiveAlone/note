# Query DSL

- Leaf 叶子节点查询
  - match,term,range 数值查询方式
- 组合查询
  - bool, dis_max 查询方式

## Query Filter Context

- Query Context
  - doc 匹配度 _score 积分计算方式
- Filter Context
  - boolean result 校验

## match_all

- 返回 _score=1, 默认 可以设置 ```boost:1.2```

## Full Text Querries

- match query, Field 字段的匹配度, _all 匹配any 字段
  - match boolean 类型查询, ```operator``` 默认 ```or``` 关系。 
  - ```minimum_should_match``` 设置最小匹配度
  - ```lenient=true``` 忽略异常, numeric 类型忽略
  - ```fuzziness``` 构建模糊查询， prefix_length 模糊查询行为
  - ```zero_terms_query``` 设置 当 所有词条,没有匹配，是否返回文档
- mathc_phrase 分析文本构建短语
  - slop 位置词 个数
  - analyzer 对应的分析器
- match_phrase_prefix
  - ```max_expansions``` 控制多少前缀将被重写成最后的词条
- multi_match 多字段 Mathc 匹配
  - best_fields 多字段匹配, 计算 匹配度高的 ```_score * tie_breaker```
  - most_filed, 匹配字段多
- Common Terms Query
  - question ```the``` 这样的词出现频率较高， 忽略又会有影响的词
  - 解决方法： 词汇分成：1 import word 出现频率较低 2 less import 出现的频率较高
    - ```cutoff_frequency``` 解决词频出现的概率低情况
- Query String Query ```query_string```  字段查询
  - query 查询的内容
  - default_operator 分词后， 词之间匹配关系
  - analyzer
  - allow_lending_wildcard 指定是否允许通配符作为词条的第一个字符，默认true
- simple query string

## Term Level queries

- term query
  - 默认 term, 匹配具体 Word, boost: x.0, 设置匹配权重
  - boost 对应数值类型, keyword 类型等. 设置搜索的匹配关联度
- range exits,
- prefix 设置 field not analyse, term 查询包含的 Term
- wildcard query 通配符匹配方式
- regexp query 正则匹配方式
- Type Ids, 通过匹配类型， ids document 搜索方式

## Compound Query 查询条件的组合方式

- Constant Score Query
  - 一些Filter 过滤条件, 设置 boost:1.2 设置过滤条件的权重
- Bool Query
  - bool 方式 映射 Luence 的Boolean Query 查询
  - must 匹配文档加入Score 计算
  - filter 不计入 Score 计算 会有Cache 缓存方式
  - should must_not
  - bool 在 query context 中, bool 中匹配成功的，* boost 计入 _score 分数中
  - bool.filter 匹配度不会影响Score 数值
    - ```constant_score```，filter 结果影响Score
- Dis Max Query
  - 多个子 Query,选取最大Score 积分， 乘以 tie_breaker 计入 总 _score 中
  - 不同于 Bool Query, 每个子查询 Score 求和方式
- function score, 函数方式影响Score
  - script score 通过脚本, 计算Score
  - weight score * weight 设置
  - ```field_value_factor```, 设置字段为 score 分数
  - 提供不同 分布函数
- Span Query 跨度查询, 查询词汇 文本中的偏移量