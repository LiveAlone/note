# Immutables 

> 开发过程中, Java 函数 final对象引用，仅仅不允许修改地址， 但是 Object 内容仍然会被修改。

> 特别多人的情况下， 很难控制别人的写法。（demo 缓存的 Bean 是不允许修改的！！） github 提供了 [Immutable](https://github.com/immutables/immutables) 不用重复造轮子了

- Values Builders, 构建 Objects, Builders, 类型安全， Null安全， 线程安全， 没有模板引用
- 简单使用： IDES 集成， 提供 not runtime 的时候依赖
- feature: lazy, derived 衍生， optional. 支持 Guava 集合。单例， 内部线程
- clean code： 简介的代码， 没有Named 等代码
- 灵活性: 适应代码惯例， 定制化 get,set,with 等。 按照需求的定制化，
- Json 的支持， 适配 Jackson Gson 的方式

## get started
- provider 方式 maven 的依赖, 通过 lombok 的方式， IDE 支持及时的编译方式。
```java
<dependency>
  <groupId>org.immutables</groupId>
  <artifactId>value</artifactId>
  <version>2.5.5</version>
  <scope>provided</scope>
</dependency>
```
-  代码的生成方式， AbstractClass, Interface, 代码生成方式
  - ImmutableXXX 生成 Class
  - 获取 getXXX() 获取属性
  - 通过 withXXX() 创建一个新 Not Update 对象。 Collection 使用的 Guava 的属性
  - Builder 的构造方式， 接口够照方式， from(interface) 因此 所有的 Bean 都要通过集成接口的方式
- 参数
  - ``` @Value.Immutable ```, ``` @Value.Parameter ``` 指定 bean 参数
  - ``` @Value.Include ```, ``` @Value.Exclude ``` value 的引用的 包含 排除
  - ``` @Value.Default ``` 设置接口 默认  Default Value
  - ``` @Value.Derived ``` 方法定义 ``` @Value.Lazy ``` 延迟执行
  - @Nullable,@AllowNulls, @SkipNulls
  - @Value.Check
  - @Value.Immutable(singleton = true) 单例
  - @Value.Auxiliary 基础 Hash 配置


