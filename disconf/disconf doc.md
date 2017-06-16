# disconf 基础文档配置

> 文档的基础内容， install 导入配置

## 目标

- 部署简单，Web 修改， 即可 RD/QA/PRD 环境完成
- 动态部署，更改配置，即使生效。
- 统一管理， 通过Web 管理 RD/QA/PRD 的配置方式。
- 一个Jar， 随动运行。

## 功能特点

- 配置统一管理， 配置项， 配置文件统一管理
- 配置 RD/QA/PRD 环境统一管理。 配置自动更新， 项目自动加载配置信息。
- 代码注解， XML 无代码侵入的方式配置。(配置下载， 主动推送的方式)

## install

1. client install (maven 导入方式)

```java
<dependency>
    <groupId>com.baidu.disconf</groupId>
    <artifactId>disconf-client</artifactId>
    <version>2.6.36</version>
</dependency>
```

1. disconf web 安装方式

## basic config

1. 注解方式，Config 变化， 自动Reload, CallBack 回掉方式， 更新配置方式。
