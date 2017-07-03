# java9 basic

## 模块化的引入 目的

1. 可靠的配置
2. 强封装
3. 模块化JDK/JRE

显示的声明模块依赖。 启动检测, 依赖缺失，立即报错。 Java9 之前运行时刻，对应的依赖缺失。
依赖查看时间：编译， 链接（jlink 提供阶段， 创建运行时刻的映像），运行。 
jlink 使用链接器， 链接时候，解决了程序运行的最佳大小

### 模块化：
- 模块： 代码（class 对应注释，枚举等）和数据（图像， 配置文件等）集合
- 模块包含的内容：
  - 所需的其他模块（或依赖于）的列表 use
  - 导出的软件包列表（其公共API），其他模块可以使用 export
  - 开放的软件包（其整个API，公共和私有）到其他反射访问模块的列表 open
  - 使用的服务列表（或使用 java.util.ServiceLoader 类发现和加载）
  - 提供的服务的实现列表
> 其实包含两块： 使用提供Class 模块。 使用提供的ServiceLoader
- java se 包含 java.*: java.base java.sql, java.xml, java.logging
    - java base 到处核心Class, java.lang, 等等
- java jdk.* 对应的API 库 开发人员的使用方式
- java javafx.* 对应 安装fx 模块。

### 模块之间的依赖
- java8 之前，没有控制包 内的可访问权限
- 依赖的使用双向的， 制定依赖
- --add-modules 提供的服务的实现列表
  - ALL-DEFAULT
  - ALL-SYSTEM
  - ALL-MODULE-PATH

### 模块的接口声明

```
[open] module <module> {
       <module-statement>;
       <module-statement>;
       ...
}
```
1. open 开放语句：一个开放模块可以将其所有软件包的反射访问授予其他模块。 你不能在open模块中再使用open语句，因为所有程序包都是在open模块中隐式打开的.
1. opens 到处包下 所有的模块。
2. exports 导出模块， 可以将包 导入指定的模块， 开放对应的
```
exports <package> to <module1>, <module2>...;
```
> 对比导出和打开语句。 导出语句允许仅在编译时和运行时访问指定包的公共API，而打开语句允许在运行时使用反射访问指定包中的所有类型的公共和私有成员
3. requires 强制依赖
```
requires <module>;
requires transitive <module>; 隐含的 依赖链传递。 多层依赖解决。
requires static <module>; 编译时候强依赖， 运行不限制。
requires transitive static <module>;
```
4. uses: service的使用者。  providers: 服务的提供者, with: 对应的实现的提供方式
``` 
uses <service-interface>;
module M {
    uses com.jdojo.prime.PrimeChecker;
}

provides <service-interface>
    with <service-impl-class1>, <service-impl-class2>...;
module P {
    uses com.jdojo.CsvParser;
    provides com.jdojo.CsvParser
        with com.jdojo.CsvParserImpl;
    provides com.jdojo.prime.PrimeChecker
        with com.jdojo.prime.generic.FasterPrimeChecker;
}
```
> java9 天假的关键字：  open, module, requires, transitive, exports, opens, to, uses, provides 和 with

### 模块描述符号
-  编译模块声明， module-info.java 文件声明对应的模块信息
- 不通过 XML JSON 格式， Class 方式， 允许声明注解方式
- 模块版本，版本增加复杂
- 模块源文件结构

java9 4
 
