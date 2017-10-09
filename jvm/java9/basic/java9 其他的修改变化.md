# java9 其他的修改变化

- 类加载器
  - 引导类加载器加载的模块是`java.base`，`java.logging`，`java.prefs`和`java.desktop`
  - 其他类似 ```javax.sql``` 等通过 平台Class 加载器
  - Class 加载过程， 找到对应的模块， 加载对应的Class

## 资源的访问



