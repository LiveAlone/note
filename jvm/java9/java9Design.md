# java9 封装模块

## 编译，链接， 运行不同的模块方式
- 展开的目录
- JAR格式
- jmod 格式
- jimage 方式

### jar 格式
- 多版本的Jar, 单个JAR包含多个JDK的类库的相同版本。 这样的JAR被称为多版本JAR
  - 一个Jar 可能使用 JDK9 JDK10 不同的类型JDK.
  - 不同的Jdk 执行不同的 Class

### jmod 格式 jmod 工具打包， jmod格式文件
- JMOD文件在编译时和链接时都被支持，运行时却不可以

### JImage, Java 9采用了整体的方法来打包运行时映像
1. 运行时映像, 发送给你的应用程序用户，而不需要下载并安装单独的JRE软件包来运行应用程序
2. Jlink JImage 方式 链接 打包方式运行