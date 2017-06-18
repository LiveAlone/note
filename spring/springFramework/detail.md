# spring detail

## spring 配置详情
1. @Autowire spring bean 解析依赖, 不会立即解析注入Bean,  在所有的Bean 初始化完成以后， 注入对应的Bean.  （Xml 中， bean 的相互依赖。 但是 通过Autowire, 不会有循环依赖的产生）
2. Nope