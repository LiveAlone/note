# java9 basic

## 模块化的引入 目的

1. 可靠的配置
2. 强封装
3. 模块化JDK/JRE

依赖查看时间：编译， 链接（jlink 提供阶段， 创建运行时刻的映像），运行。 
Java9 之前运行时刻，对应的依赖缺失。

### 模块化：
模块： 代码（class 对应注释，枚举等）和数据（图像， 配置文件等）集合

### 模块的接口声明

```
[open] module <module> {
       <module-statement>;
       <module-statement>;
       ...
}
```
1. open 开放语句：一个开放模块可以将其所有软件包的反射访问授予其他模块。 你不能在open模块中再使用open语句，因为所有程序包都是在open模块中隐式打开的
2. export 导出模块， 可以将包 导入指定的模块， 开放对应的
```
exports <package> to <module1>, <module2>...;
```
> 对比导出和打开语句。 导出语句允许仅在编译时和运行时访问指定包的公共API，而打开语句允许在运行时使用反射访问指定包中的所有类型的公共和私有成员
3. require 强制依赖
```
requires <module>;
requires transitive <module>; 隐含的 依赖链传递。
requires static <module>; 编译时候强依赖， 运行不限制。
requires transitive static <module>;
```
4 use providers 对应的接口， 实习Class 的暴露。
```java
module P {
    uses com.jdojo.CsvParser;
    provides com.jdojo.CsvParser
        with com.jdojo.CsvParserImpl;
    provides com.jdojo.prime.PrimeChecker
        with com.jdojo.prime.generic.FasterPrimeChecker;
}
```
