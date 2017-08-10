# blog 基础

## 自动配置时候, 移除不必要的依赖配置

- ```@EnableAutoConfiguration(exclude={DataSourceAutoConfiguration.class})```
- ```@SpringBootApplication(exclude = {DataSourceAutoConfiguration.class})```
- 配置文件 exclude ```spring.autoconfigure.exclude=org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration```

## maven git commit 插件， 通过 Actuator 方式 监控当先版泵Git 的 提交状态。 .git 获取

## metrics 节点的聚合监控方式, 

- spring-boot-admin 监控
  - 缺点： 不是时间轴监控, 不同的节点， 没有聚合数据
- jconsole, visualvm 监控来自 JMX MBean, 数据. jolokia MBeans 通过RESTFUL的 方式， 监控暴露的节点
- demo
  - jolodia 导出JMX 的工具
  - Telegraf 集成JMX 的数据
  - InlfuxDB 时间序列的 数据
  - Grafana 数据源的展示

## springBoot 1.5.1 logger 的日志级别动态调整

## 传统Spring 项目 引入Actuator

- pom 依赖

```java
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-actuator</artifactId>
    <version>1.4.3.RELEASE</version>
    <type>jar</type>
</dependency>
<dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-validator</artifactId>
    <version>4.3.2.Final</version>
</dependency>
```

- MyConfiguration 配置Class 添加Bean的 注解配置方式

## spring boot banner

- banner.txt
- 继承Banner, 自定义输出的内容格式
  - ${AnsiColor.BRIGHT_RED}：设置控制台中输出内容的颜色
  - ${application.version}：用来获取MANIFEST.MF文件中的版本号
  - ${application.formatted-version}：格式化后的${application.version}版本信息
  - ${spring-boot.version}：Spring Boot的版本号
  - ${spring-boot.formatted-version}：格式化后的${spring-boot.version}版本信息

## actuaor 提供基础

- 应用配置
  - beans, env, info 等 APP 相关的JVM 信息